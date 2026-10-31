---
title: HyperFrames 渲染协议 — 手写 seek(t)，禁动画库
status: permanent
tags:
  - hyperframes
  - playwright
  - 渲染管线
  - engineering
date_created: 2026-10-28
source: internal验证（9 条商品卡 + 9 条 viral 视频）
---

# HyperFrames 渲染协议 — 手写 seek(t)，禁动画库

## 一句话解释

HF 渲染管线的铁律：通过 `window.__hf = {duration, seek(t)}` 注入动画引擎，所有元素位置/透明度必须由你手写的 `seek(t)` 函数计算。**绝对禁止** GSAP timeline / anime.js / CSS animation 等任何动画库 — Playwright 逐帧截图时动画库的时间线不走 wall-clock，造成画面撕裂和跳帧。

## 展开

### 为什么重要？

HF 的工作流是：Playwright 打开网页 → 从 t=0 到 t=duration 逐帧调 `seek(t)` → `page.screenshot()` 捕获 → ffmpeg 合成 MP4。

```
正确：t=0.0 → seek(0.0) → screenshot → t=0.033 → seek(0.033) → screenshot
错误：GSAP timeline 播放 → 截图间隔不对 → 第 5 帧还是第 3 帧的内容
```

### 三大铁律

1. **手写 seek(t)**：`function seek(t) { fadeIn = t < 0.5 ? t/0.5 : 1; ... }`
2. **Puppeteer page.screenshot**（禁 CDP captureScreenshot — 渲染不同步）
3. **强制 reflow**：每帧调 `document.body.offsetHeight` 避免浏览器优化掉 DOM 更新

### 边界 / 常见误解

- 不是禁用所有 CSS — `transition` 可以存在但不在 timeline 意义上使用
- CSS `--progress` 变量配合 seek 更新是允许的（更新变量即更新 CSS）
- `(await browser.pages())[0]` 优于 `newPage()`（沙箱差异）

### 应用场景

- 商品卡动画视频（9 条 × 4.5s）
- AI 带货视频时间线（痛点 → 品牌 → 品名 → 卖点卡 → 价格 → CTA）
- FDE 知识视频（墨水黑 + 信号橙美学）

## 出站链接

- [[Playwright SwiftShader 黑帧修复]] — 渲染管线的另一个坑
- [[漫剧工作流 — HyperFrames 商业卡渲染]] — 应用场景

## 来源

- 2026-10-28 本机验证：1280×720 · 30fps · 商品卡 4.5s + 爆款视频 15s
