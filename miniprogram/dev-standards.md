# 微信小程序开发规范

> 适用于团队所有小程序项目。基于「简压照片缩图」踩坑经验总结。

## 1. 尺寸与适配

### 必须用 rpx 的场景
- 所有布局尺寸：padding、margin、gap、width、height
- 字号：font-size
- 圆角：border-radius（大圆角）
- 换算规则：**1px = 2rpx**（基于 375pt 设计稿）

### 必须用 px 的场景
- 自定义导航栏高度（`wx.getSystemInfoSync().statusBarHeight` 返回 px）
- `wx.getMenuButtonBoundingClientRect()` 返回值
- 1px 细线边框（`border-width: 1px`）
- `letter-spacing` 等微调属性

### 禁止
- ❌ rpx 和 px 混用在同一个布局计算中
- ❌ 固定 px 做主要布局间距

## 2. 图标方案

### 推荐（按优先级）
1. **emoji** — 最稳，所有机型都支持，零兼容性问题
2. **本地 PNG base64** — `data:image/png;base64,...`，image 组件支持好
3. **本地 PNG 文件** — `assets/icon.png`，相对路径引用

### 禁止
- ❌ SVG data URI（`data:image/svg+xml,...`）— 真机渲染不稳定
- ❌ 远程图片 URL 做核心图标 — 离线场景无法显示

## 3. 自定义导航栏

```javascript
// app.js — 标准写法
onLaunch() {
  const sysInfo = wx.getSystemInfoSync()
  this.globalData.statusBarHeight = sysInfo.statusBarHeight || 47

  try {
    const menuBtn = wx.getMenuButtonBoundingClientRect()
    this.globalData.menuButtonInfo = menuBtn
    this.globalData.navBarHeight = (menuBtn.top - sysInfo.statusBarHeight) * 2 + menuBtn.height
  } catch (e) {
    this.globalData.navBarHeight = 44
  }
}
```

### 注意事项
- `navigationStyle: custom` 写在 `app.json` 的 `window` 里
- 导航栏相关样式统一用 **px**（和系统 API 返回值一致）
- 自定义导航栏不能遮住右上角胶囊按钮
- 首页如果不需要导航栏，用 `disableScroll: true` 禁止滚动

## 4. 按钮样式重置

小程序 `<button>` 有大量默认样式，必须全面重置：

```css
/* app.wxss — 全局按钮重置 */
button {
  margin: 0;
  padding: 0;
  line-height: 1;
  border: none;
  background: transparent;
}

button::after {
  border: none;
}
```

### 按钮居中
- 用 `display: flex; align-items: center; justify-content: center`
- ❌ 不要依赖 `line-height` 和按钮高度匹配来居中

## 5. 安全区适配

```css
/* 底部安全区（刘海屏/全面屏） */
padding-bottom: calc(24rpx + constant(safe-area-inset-bottom));
padding-bottom: calc(24rpx + env(safe-area-inset-bottom));
```

- `constant()` 和 `env()` 双写，兼容 iOS 11.0-11.2 和更高版本
- 底部固定栏必须加安全区

## 6. 页面配置

每个页面必须有 `.json` 配置文件：

```json
{
  "usingComponents": {},
  "disableScroll": false
}
```

- 首页（一屏内容）：`"disableScroll": true`
- 使用 scroll-view 管理滚动的页面：`"disableScroll": true`（防止双重滚动）

## 7. 数据传递

- 页面间传数据用 `app.globalData`，不用 URL 参数（避免长度限制）
- ❌ 不要把 base64 图片数据放进 `setData`（性能杀手）
- 图片一律用 `tempFilePath` 路径

## 8. 性能与内存

- 批量操作（如批量压缩）必须串行处理，避免内存峰值
- 离屏 canvas 用完必须释放：`ctx.clearRect(0, 0, w, h)`
- 大文件阈值：>10MB 提示/自动处理，>20MB 拒绝
- `wx.showLoading` 在异步操作开始前调用，完成后 `wx.hideLoading`

## 9. 兼容性

- 基础库最低版本：2.26.0（`app.json` 不直接配置，提交审核时设置）
- `backdrop-filter` 在低端安卓不生效 — 用 `rgba()` 半透明背景兜底
- `createOffscreenCanvas` 需要真机验证，模拟器行为可能不一致
- API 能力检测：`wx.canIUse('xxx')` 再调用

## 10. 项目结构模板

```
miniprogram/
├── app.js                    # 全局逻辑 + globalData
├── app.json                  # 路由 + window 配置
├── app.wxss                  # 设计系统变量 + 全局组件样式 + 按钮重置
├── project.config.json       # 项目配置（AppID 等）
├── sitemap.json              # 小程序 SEO
├── utils/                    # 工具函数
│   └── xxx.js
├── assets/                   # 本地图片资源（PNG）
└── pages/
    └── xxx/
        ├── xxx.js
        ├── xxx.json          # 每个页面必须有
        ├── xxx.wxml
        └── xxx.wxss
```

## 11. Code Review Checklist

Developer 提交代码后，Architect 按此清单检查：

- [ ] 所有布局尺寸使用 rpx（导航栏除外）
- [ ] 图标使用 emoji 或 PNG base64，无 SVG data URI
- [ ] `<button>` 默认样式已重置
- [ ] 底部固定栏有 safe-area 适配
- [ ] 每个页面有 `.json` 配置文件
- [ ] 需要固定一屏的页面设置了 `disableScroll: true`
- [ ] 数据传递用 globalData，无 base64 进 setData
- [ ] 离屏 canvas 用完已释放
- [ ] 大文件有阈值保护
- [ ] `wx.showLoading` / `wx.hideLoading` 成对出现
- [ ] 错误处理完整（try-catch / fail 回调）
- [ ] 权限拒绝有引导 `wx.openSetting`

## 12. 提审前检查

- [ ] 微信开发者工具代码质量检查全绿
- [ ] `app.json` 已配置 `"lazyCodeLoading": "requiredComponents"`
- [ ] 无未使用的文件（assets 清理）
- [ ] 项目包大小 < 2MB
- [ ] 10 轮 QA 测试通过，零 P0/P1 bug

---

*文档维护：Architect | 最后更新：2026-03-14*
