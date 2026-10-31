---
title: Windows LongPathsEnabled 修 pip 260 字符地狱
status: permanent
tags:
  - windows
  - pip
  - python
  - 环境踩坑
  - engineering
date_created: 2026-10-27
source: MoneyPrinterTurbo 安装过程
---

# Windows LongPathsEnabled 修 pip 260 字符地狱

## 一句话解释

Windows 默认限制文件路径最大 260 字符（MAX_PATH），而 npm/pip 包的嵌套依赖动辄 300+ 字符。启用 `LongPathsEnabled` 注册表键后，Windows API 取消此限制，让 pip 能正确安装如 litellm 这种深度嵌套包。

## 展开

### 为什么重要？

这几乎是所有 Python/Node.js 开发在 Windows 上的必经之坑。症状：

```
ERROR: Could not install packages due to an OSError: [Errno 2] No such file or directory:
'C:\Users\Administrator\AppData\Local\Pip\Cache\...\<很长的包名>\...\<更深>\setup.py'
```

表面看是"文件不存在"，实际是路径太长，Windows API 直接拒绝创建。

### 修复方法

```powershell
# PowerShell (管理员)
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
  -Name "LongPathsEnabled" -Value 1

# 或通过 gpedit.msc → 计算机配置 → 管理模板 → 系统 → 文件系统 → 启用 Win32 长路径
```

### 注意

- 注册表改完**立即生效**，无需重启
- 但 pip 缓存中已有的**损坏包需清理**：`pip cache purge`
- 清理后 `pip install --force-reinstall --no-deps <包名>` 重装
- 有此设置后 Python 3.6+ 也是默认支持的

### 边界 / 常见误解

- 不是只影响 Python — npm, pnpm, cargo 等在 Windows 上都受 260 字符限制
- 不是所有程序都遵守注册表设置 — 部分旧应用硬编码 MAX_PATH
- `\\?\` 前缀也可以绕过限制（但 pip 内部不支持）

### 应用场景

- MoneyPrinterTurbo 安装（litellm 包嵌套超深）
- 任何 pip install 报 `OSError: [Errno 2]` 但文件实际上存在/曾经存在
- 多依赖项目（Django / FastAPI 等大型框架在 Windows 上）

## 出站链接

- [[ComfyUI 的 PyTorch 版本锁]] — 同一个安装日踩的另一个坑
- [[漫剧工作流 ComfyUI + MoneyPrinterTurbo 工具链]]

## 来源

- 2026-10-27 本机: MoneyPrinterTurbo litellm 安装报错 → 修 LongPathsEnabled = 1
