---
title: ComfyUI 的 PyTorch 版本锁 — 低于 2.7 会静默崩
status: permanent
tags:
  - comfyui
  - pytorch
  - 环境踩坑
  - engineering
date_created: 2026-10-27
source: internal验证（GTX 1660 6GB + driver 560.94）
---

# ComfyUI 的 PyTorch 版本锁 — 低于 2.7 会静默崩

## 一句话解释

ComfyUI 0.43+ 依赖 `comfy-kitchen>=0.2.40`，而该包用到了 torch 2.7 才有的 `infer_schema` 特性来支持 `list[int]` 参数类型。装 torch 2.6 及以下版本，节点注册阶段不会报错，但推理调用时会直接崩溃。

## 展开

### 为什么重要？

这个坑的特殊性在于**不报错**。你会看到 ComfyUI 正常启动、WebUI 正常加载、节点全部显示，但一跑图就崩。排查链条非常长：

```
torch 崩溃 → 容易被误判为显存不足 / 模型损坏 / 采样器错误
           → 浪费大量时间在错误方向排查
```

### 边界 / 常见误解

- 不是 CUDA 版本的问题（用 CUDA 12.4 或 12.6 都行）
- 不是 comfy-kitchen 装错了（装任何版本都会触发同一个底层问题）
- 不是模型文件损坏（模型在 SD WebUI 下能正常跑）
- **唯一修复**：torch >= 2.7 + comfy-kitchen >= 0.2.40 + Python 3.12

### 应用场景

- 新机器搭建 ComfyUI → 先确认 torch 版本再装其他
- ComfyUI 升级后突然崩了 → 检查 torch 是否被 pip 降级
- 教程里 `pip install torch` 默认装 2.6 → 手动指定 2.7.1+cu126

## 出站链接

- [[漫剧工作流的画面生成层 — ComfyUI SD 1.5]] — 这是 ComfyUI 的使用场景
- [[LongPathsEnabled 修 Windows 260 字符限制]] — 另一个 Windows 环境坑

## 来源

- 2026-10-27 本机验证：torch 2.7.1+cu126 + comfy-kitchen 0.2.40 + SD 1.5 稳定出图
