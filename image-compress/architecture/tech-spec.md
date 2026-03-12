# 图片压缩微信小程序 — 技术架构文档

## 版本信息
- **版本**: v1.0
- **作者**: Architect
- **日期**: 2026-03-12
- **基于**: PM PRD v1.0

---

## 一、技术选型

| 项 | 选择 | 理由 |
|-----|------|------|
| 开发框架 | 微信原生小程序 | 3 个功能页，无需跨端框架 |
| 压缩方案 | wx.compressImage + canvas 2D | 原生 API 优先，canvas 兜底 PNG |
| 选图 API | wx.chooseMedia | wx.chooseImage 已废弃（基础库 2.21.0） |
| 存储 | wx.setStorageSync | 本地存储，不需要后端 |
| 广告 SDK | 微信流量主原生组件 | ad / rewarded-video-ad |
| 基础库最低版本 | 2.26.0 | 需要 compressedWidth/compressedHeight 参数 |

---

## 二、项目结构

```
image-compress/
├── app.js                    # 应用入口
├── app.json                  # 全局配置（页面路由、窗口、tabBar）
├── app.wxss                  # 全局样式（极简设计系统）
├── project.config.json       # 项目配置
├── sitemap.json              # 小程序搜索 sitemap
│
├── pages/
│   ├── index/                # 首页（选图/拍照入口）
│   │   ├── index.wxml
│   │   ├── index.wxss
│   │   └── index.js
│   │
│   ├── compress/             # 压缩设置页（预览+档位选择）
│   │   ├── compress.wxml
│   │   ├── compress.wxss
│   │   └── compress.js
│   │
│   ├── result/               # 结果页（对比+保存）
│   │   ├── result.wxml
│   │   ├── result.wxss
│   │   └── result.js
│   │
│   └── about/                # 关于页
│       ├── about.wxml
│       ├── about.wxss
│       └── about.js
│
├── utils/
│   ├── compress.js           # 压缩核心逻辑（封装两种方案）
│   └── util.js               # 工具函数（formatBytes 等）
│
└── assets/
    └── icons/                # 图标资源（SVG 转 base64 内联）
```

---

## 三、核心模块设计

### 3.1 压缩引擎（utils/compress.js）

```
模块导出：
  compressImage(filePath, quality, targetWidth?, targetHeight?)
    → Promise<{ tempFilePath, originalSize, compressedSize }>

  compressBatch(fileList, quality, onProgress)
    → Promise<results[]>
```

**压缩策略：**

```
输入图片
  ↓
判断格式（通过文件扩展名 + wx.getImageInfo）
  ├── JPG/JPEG → wx.compressImage({ src, quality, compressedWidth?, compressedHeight? })
  └── PNG/其他  → canvas 2D 方案
                   1. wx.getImageInfo 获取宽高
                   2. 创建离屏 canvas（wx.createOffscreenCanvas）
                   3. drawImage 绘制（按目标尺寸缩放）
                   4. canvas.toDataURL('image/jpeg', quality/100) 或
                      wx.canvasToTempFilePath({ quality })
                   5. 返回临时文件路径
  ↓
wx.getFileInfo 获取压缩后文件大小
  ↓
返回 { tempFilePath, originalSize, compressedSize }
```

**关键实现细节：**
- PNG 压缩后输出为 JPG（有损压缩才有效果，PNG 无损压缩率很低）
- 如果用户需要保持 PNG 透明度 → V1.1 再考虑，MVP 统一输出 JPG
- canvas 操作在离屏 canvas 上进行，不影响 UI 渲染
- 大图（>10MB）先缩尺寸再压质量，避免内存溢出

### 3.2 批量压缩

```
compressBatch(fileList, quality, onProgress):
  for (i = 0; i < fileList.length; i++):
    result = await compressImage(fileList[i], quality)
    onProgress(i + 1, fileList.length, result)
    results.push(result)
  return results
```

- 串行处理，不并行（避免内存峰值过高）
- 每完成一张回调 onProgress，UI 更新进度（1/5, 2/5...）

---

## 四、页面数据流

### 4.1 首页（pages/index）

```
用户操作：
  点击"选择图片" → wx.chooseMedia({ count: 9, mediaType: ['image'], sourceType: ['album'] })
  点击"拍照"     → wx.chooseMedia({ count: 1, mediaType: ['image'], sourceType: ['camera'] })

  ↓ 获取 tempFiles[]（含 tempFilePath, size）
  ↓ 立即调用 compressBatch(tempFiles, 60, onProgress)  // 默认标准档自动压缩
  ↓ 压缩完成后跳转结果页

  wx.navigateTo({ url: '/pages/result/result' })
  数据传递：app.globalData = { tempFiles, results }
```

### 4.2 结果页（pages/result）— 合并了压缩设置 + 结果展示

```
onLoad:
  从 app.globalData 读取 results（已压缩完成）
  展示每张图片：缩略图 + 原始大小 → 压缩大小、压缩率
  展示总计：总原始大小 → 总压缩大小
  当前档位高亮"标准"

用户操作：
  切换档位（轻度 80% / 标准 60% / 深度 40%）
    → 重新调用 compressBatch(tempFiles, newQuality, onProgress)
    → 实时更新压缩结果

  点击"保存到相册" → wx.saveImageToPhotosAlbum({ filePath })
    ↓ 需要授权 scope.writePhotosAlbum
    ↓ 首次弹授权框，用户拒绝后引导到设置页
    ↓ 保存成功 → wx.showToast({ title: '已保存', icon: 'success' })

  点击"继续压缩" → wx.navigateBack 回首页

  底部：Banner 广告位 <ad unit-id="..." />
```

---

## 五、权限处理

| 权限 | 场景 | 处理方式 |
|------|------|---------|
| scope.writePhotosAlbum | 保存图片到相册 | 首次自动弹窗；拒绝后点保存时 wx.openSetting 引导 |
| scope.camera | 拍照 | wx.chooseMedia 自动处理 |
| scope.album | 选相册图片 | wx.chooseMedia 自动处理 |

**权限拒绝兜底：**
```
try {
  await wx.saveImageToPhotosAlbum({ filePath })
} catch (e) {
  if (e.errMsg.includes('deny') || e.errMsg.includes('auth')) {
    wx.showModal({
      title: '需要相册权限',
      content: '请在设置中允许保存图片到相册',
      confirmText: '去设置',
      success: (res) => { if (res.confirm) wx.openSetting() }
    })
  }
}
```

---

## 六、性能控制

### 6.1 内存安全

| 场景 | 策略 |
|------|------|
| 单张 >10MB | 先缩尺寸到 2000px 宽，再压质量 |
| 单张 >20MB | 提示"图片过大"，拒绝处理 |
| 批量 9 张 | 串行处理 + 每张处理后释放上一张的临时资源 |
| canvas 离屏 | 用完立即 canvas = null 释放 |

### 6.2 setData 优化

- **禁止**把图片 base64 放入 setData
- 所有图片展示用 `<image src="{{tempFilePath}}" />` 临时路径
- setData 只传状态数据（进度、文件大小数字、当前档位）
- 批量压缩时用 `setData({ 'results[' + i + ']': result })` 局部更新

### 6.3 包体积控制

- 预估总包 <200KB（4 页 WXML/WXSS/JS + 工具函数）
- 无第三方依赖
- 图标用内联 SVG base64 或小程序原生 icon 组件
- 远低于 2MB 主包限制

---

## 七、广告位设计

| 广告类型 | 位置 | 触发条件 |
|----------|------|---------|
| Banner 广告 | 结果页底部 | 压缩完成后展示 |
| 激励视频（V1.1） | 批量压缩 >3 张时 | 可选：看广告解锁无限批量 |

**注意：流量主需 1000 UV 后才能开通，MVP 先预留广告位组件，审核通过后再填 unit-id。**

```wxml
<!-- 结果页底部 -->
<ad wx:if="{{showAd}}" unit-id="{{adUnitId}}" ad-type="banner" />
```

---

## 八、app.json 配置

```json
{
  "pages": [
    "pages/index/index",
    "pages/compress/compress",
    "pages/result/result",
    "pages/about/about"
  ],
  "window": {
    "navigationBarTitleText": "极简压图",
    "navigationBarBackgroundColor": "#ffffff",
    "navigationBarTextStyle": "black",
    "backgroundColor": "#f5f5f5"
  },
  "style": "v2",
  "sitemapLocation": "sitemap.json"
}
```

---

## 九、风险 & 注意事项

| 风险 | 影响 | 缓解措施 |
|------|------|---------|
| iOS wx.compressImage 不支持 PNG | PNG 图片无法压缩 | canvas 2D 兜底方案 |
| PNG → JPG 丢失透明度 | 透明背景变白 | MVP 接受；V1.1 提供 PNG→PNG 选项 |
| 大图内存溢出 | 小程序闪退 | >10MB 先缩尺寸，>20MB 拒绝 |
| 个人主体审核 | 可能被拒 | 类目选"工具 > 图片/视频"或"信息查询" |
| 基础库 2.26.0 要求 | 老版本微信不支持 compressedWidth | 做 wx.canIUse 检查，不支持则不传尺寸参数 |

---

## 十、开发检查清单（给 Developer）

- [ ] 初始化小程序项目（微信开发者工具）
- [ ] app.json 页面路由配置
- [ ] 全局样式（极简设计系统 — 等 Designer 出稿）
- [ ] 首页：选图 + 拍照
- [ ] 压缩设置页：预览 + 三档选择 + 开始按钮
- [ ] utils/compress.js：wx.compressImage + canvas 兜底
- [ ] 结果页：大小对比 + 压缩率 + 保存按钮
- [ ] 批量压缩 + 进度显示
- [ ] 权限处理（相册保存授权 + 拒绝引导）
- [ ] 大图保护（>10MB 缩尺寸，>20MB 拒绝）
- [ ] setData 优化（不传 base64）
- [ ] 关于页
- [ ] 广告位预留
- [ ] 真机测试（iOS + Android）

---

*Architect 🏗️ — 2026-03-12*
