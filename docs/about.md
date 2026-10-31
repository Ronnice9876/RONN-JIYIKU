---
title: 关于这个知识库
description: 方法论、技术栈、使用说明
status: seed
tags:
  - 元
  - about
---

# ℹ️ 关于这个知识库

## 📌 这个知识库做什么？

这个仓库是我的**长期知识操作系统**，用于把零散的信息转化为可检索、可复用、可增长的结构化知识。

它不是一个「整理好就会用」的档案库，而是一个**让知识自己长出来**的生态。

## 🧬 设计哲学

```
种子 (Seed)  →  萌芽 (Sprout)  →  常青 (Evergreen)
  播种期         维护期            收获期
```

| 原则 | 含义 |
|------|------|
| **原子化** | 一张卡片只讲一件事 |
| **链接优先** | 用双向链接串联而非依赖目录层级 |
| **渐进披露** | 先骨架后细节，不追求一步到位 |
| **使用驱动** | 每个知识点标注「什么时候会被唤起」 |

## 🛠️ 技术栈

| 层 | 技术 | 说明 |
|------|------|------|
| 写作 | Markdown | 人类可读、版本可控 |
| 结构化 | YAML frontmatter | 元数据管理 |
| 生成 | MkDocs Material | 静态站点，Material Design |
| 搜索 | MkDocs Material 内置 | 客户端全文搜索，无需后端 |
| 托管 | GitHub Pages | 免费、全球 CDN |
| 部署 | GitHub Actions | 推送即部署 |
| 版本 | Git | 完整历史，随时回滚 |

## 📁 目录结构

```
knowledge-base/
├── mkdocs.yml              # 全局配置
├── docs/                   # 所有 Markdown 文档
│   ├── index.md            # 首页
│   ├── domains/            # 一级知识领域（6 个）
│   │   ├── cs/             # 计算机科学
│   │   ├── engineering/    # 工程/技术栈
│   │   ├── math/           # 数学/统计
│   │   ├── design/         # 设计/美学
│   │   ├── business/       # 商业/经济
│   │   └── growth/         # 个人成长
│   ├── cards/              # 卡片盒 (Zettelkasten)
│   │   ├── atoms/          # 原子卡片索引
│   │   └── permanent/      # 永久笔记池
│   ├── resources/          # 资源
│   │   ├── books.md        # 书单
│   │   ├── tools.md        # 工具清单
│   │   └── links.md        # 网址收藏
│   ├── journal/            # 日志
│   │   ├── weekly/         # 周记
│   │   └── reading/        # 读书笔记
│   ├── templates/          # 写作模板
│   │   ├── card.md         # 知识卡片模板
│   │   ├── reading.md      # 读书笔记模板
│   │   └── meeting.md      # 会议纪要模板
│   ├── stylesheets/        # 自定义 CSS
│   ├── javascripts/        # 自定义 JS
│   └── about.md            # 本文件
└── .github/
    └── workflows/
        └── deploy.yml      # GitHub Actions 自动部署
```

## 🚀 部署到 GitHub Pages

### 初次设置

```bash
# 1. 在 GitHub 创建新仓库 knowledge-base
# 2. 关联并推送
git init
git remote add origin https://github.com/<你的用户名>/knowledge-base.git
git add .
git commit -m "init: 知识库基础结构"
git push -u origin main

# 3. 首次部署（首次生成 gh-pages 分支）
mkdocs gh-deploy

# 4. 在 GitHub 仓库 Settings → Pages → Source 选择 gh-pages 分支
```

### 日常维护

```bash
# 写新内容 → 提交 → 自动部署
git add .
git commit -m "add: [描述]"
git push
mkdocs gh-deploy
# 或者直接用 GitHub Actions 自动部署（无需手动 gh-deploy）
```

## 📜 许可

- 内容：CC BY-NC-SA 4.0（非商用）
- 代码/配置：MIT License

---

> 「知识管理的最高境界，不是记住一切，而是知道去哪里找到一切。」
