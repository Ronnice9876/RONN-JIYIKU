---
title: 工程/技术栈
description: 实战技术经验与工作流
status: sprout
tags:
  - domain-engineering
  - 实战
---

# 🔧 工程/技术栈

> 落地为王。所有不转为可运行代码的理论都是草稿。

## 📌 工程师核心能力栈

| 层级 | 能力 | 典型工具 |
|------|------|---------|
| L1 使用 | 用 API / 库搭解决方案 | Python, Node.js, pip, npm |
| L2 集成 | 多工具串联、端到端工作流 | Docker, GitHub Actions |
| L3 设计 | 系统架构、数据流设计 | UML, Mermaid, System Design |
| L4 优化 | 性能调优、成本优化 | Profiling, A/B Testing |

## 🗂️ 工程日志（按时间线）

```text
2026-10 [漫剧工作流六件套] drama-pipeline / director-style / character-consistency
            / multiref-shot / storyboard-canvas / action-choreo
         — 完整跑通「小说→分镜→出图→合成」全链路
         — 工具链：ComfyUI SD 1.5 + edge-tts + ffmpeg + AI 视频

2026-10 [HyperFrames 渲染管线] Playwright + ffmpeg 逐帧录制
         — HF 协议：window.__hf = {duration, seek}
         — 9 条商品卡 + 9 条 AI 带货视频

2026-10 [AI 多智能体引擎 — 女娲 Nvwa] 5 Agent 协作内容生产
         — 架构：Fastify SSE + ComfyUI + Seedance + edge-tts
```

## 🔌 已验证工具链

```
ComfyUI     → 画面生成（SD 1.5, 控制网）
MoneyPrinterTurbo → 自动成片（streamlit 8501）
edge-tts    → 旁白配音（YunxiNeural）
ffmpeg      → 视频合成/编码
Playwright  → 浏览器自动化渲染
```

## ⚠️ 踩坑清单

| 场景 | 坑 | 解法 |
|------|-----|------|
| ComfyUI | torch 2.6 以下版本崩溃 | 需 ≥2.7 + comfy-kitchen≥0.2.40 |
| 大模型下载 | 国内网速慢 | hf-mirror + hf-transfer |
| litellm 安装 | Windows 路径超 260 字符 | 注册表 LongPathsEnabled=1 |
| Playwright | SwiftShader 冷启动黑帧 | warmup 5 轮 + enable-unsafe-swiftshader |
