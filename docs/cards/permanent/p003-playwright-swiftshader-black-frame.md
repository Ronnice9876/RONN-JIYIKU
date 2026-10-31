---
title: Playwright SwiftShader 黑帧 — GL 冷启动需 warmup
status: permanent
tags:
  - playwright
  - swiftshader
  - 渲染管线
  - engineering
date_created: 2026-10-28
source: internal验证（FDE 知识视频 1280×720）
---

# Playwright SwiftShader 黑帧 — GL 冷启动需 warmup

## 一句话解释

Playwright headless 模式下用 SwiftShader 做软件渲染（没 GPU 也能跑），但 Chromium GL 冷启动时前几帧会渲染成全黑。warmup 至少 5 轮空白截图 + `setTimeout(30)` 后置延迟可彻底消除。

## 展开

### 为什么重要？

如果你用 Playwright + ffmpeg 逐帧录制视频，但系统没有独显/独显没驱动，就只能走 SwiftShader 软件渲染。冷启动时：

```
第 1 帧: 纯黑 (GL 还没初始化完)
第 2 帧: 纯黑 (buffer swap 未完成)
第 3 帧: 可能是黑可能是残影
第 4 帧起: 正常
```

对 30fps 视频来说，前 3 帧就是 100ms 黑帧 — 在知识视频、产品展示中非常扎眼。

### 修复方案

```javascript
// Launch flags
args: ['--use-gl=angle', '--use-angle=swiftshader', '--enable-unsafe-swiftshader', '--ignore-gpu-blocklist']

// Warmup
for (let i = 0; i < 5; i++) {
  await page.screenshot(); // 丢掉的帧
}
await new Promise(r => setTimeout(r, 30));
// 开始正式录制
```

### 边界 / 常见误解

- 不只是 Chromium — Chrome headless 也有同样问题
- `--enable-unsafe-swiftshader` 是必须 flag，否则软件渲染被 blocklist 禁用
- 30ms 延迟在某些机器上不够，调到 100ms 更安全
- 软件渲染速度约 5.5fps（SwiftShader），90s 视频全片渲染约 8-10 分钟

### 应用场景

- pdoom-video 离线渲染
- HyperFrames 商品卡录制
- 没有 GPU 的机器上跑端到端自动化测试

## 出站链接

- [[HyperFrames 渲染协议 — 手写 seek(t)]] — 协议层
- [[漫剧工作流 — 渲染管线]] — 应用层

## 来源

- 2026-10-28 本机验证：GTX 1660 但用 SwiftShader 纯软件渲染 1280×720
