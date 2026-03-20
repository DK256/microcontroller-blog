---
name: hexo-blog-setup
overview: 初始化 Hexo 博客完整配置：安装 NexT 主题、创建 Hexo 目录结构、配置 _config.yml、配置搜索/RSS/部署插件、创建 ESP32/ESP8266 首篇文章，最终可通过 `hexo server` 本地预览。
todos:
  - id: init-hexo-structure
    content: 创建 Hexo 标准目录结构（scaffolds/、source/、themes/）和基础配置文件 _config.yml
    status: completed
  - id: install-next-theme
    content: 通过 Git Submodule 安装 NexT 主题并创建 _config.next.yml 主题配置
    status: completed
    dependencies:
      - init-hexo-structure
  - id: create-pages
    content: 创建关于页面、分类页面、标签页面
    status: completed
    dependencies:
      - init-hexo-structure
  - id: configure-plugins
    content: 配置插件（搜索、RSS、Markdown-it）并整合到主题配置中
    status: completed
    dependencies:
      - install-next-theme
  - id: configure-deploy
    content: 配置 hexo-deployer-git 部署到 GitHub Pages
    status: completed
    dependencies:
      - install-next-theme
  - id: create-first-post
    content: 创建第一篇 ESP32 入门文章模板
    status: completed
    dependencies:
      - configure-plugins
---

## 产品概述

搭建一个基于 Hexo 框架的个人技术博客，专注于 ESP32/ESP8266 单片机开发主题，使用 NexT 主题，部署至 GitHub Pages。

## 核心功能

- **Hexo 博客初始化**：创建完整的 Hexo 目录结构（`_config.yml`、`scaffolds/`、`source/`、`themes/`）
- **NexT 主题集成**：安装并配置 NexT v8.x 主题，启用搜索、RSS 订阅、本地搜索等功能
- **插件配置**：配置 hexo-generator-searchdb（本地搜索）、hexo-generator-feed（RSS）、hexo-renderer-markdown-it（Markdown 增强）
- **GitHub Pages 部署**：配置 hexo-deployer-git 实现一键部署到 GitHub Pages
- **博客基础内容**：创建"关于"页面、分类/标签系统、以及第一篇 ESP32 入门文章模板

## 技术栈

- **博客框架**：Hexo 8.1.1（静态站点生成器）
- **主题**：hexo-theme-next（NexT v8.x）
- **部署**：hexo-deployer-git → GitHub Pages
- **Markdown 渲染**：hexo-renderer-markdown-it（支持扩展语法）
- **插件**：hexo-generator-searchdb、hexo-generator-feed

## 实现方案

### 策略

由于项目不是通过 `hexo init` 初始化的（当前只有 `package.json` 和 `README.md`），需要手动创建 Hexo 完整目录结构和配置文件。同时将 NexT 主题作为 Git submodule 引入，便于后续版本更新。

### 关键决策

1. **NexT 主题引入方式**：通过 Git Submodule 克隆 `https://github.com/next-theme/hexo-theme-next` 到 `themes/next/`，而非 npm 安装，因为 NexT v8 推荐使用 Git 方式管理，配置更灵活且支持多主题方案切换
2. **NexT 主题配置方式**：使用 NexT v8 推荐的**主从配置模式**——主配置 `_config.yml` 通过 `theme_config` 或 `_config.next.yml` 覆盖主题默认配置，保持主题可升级性
3. **部署策略**：使用独立的部署分支（如 `gh-pages`），源码保留在 `main` 分支，实现源码与静态文件分离
4. **Markdown-it 渲染器**：配置启用脚注、下标、上标、任务列表等扩展功能，适合技术文档写作

### 性能与可靠性

- `hexo generate` 生成静态文件速度快，对技术博客规模（< 1000 篇文章）无性能瓶颈
- 本地搜索通过 `hexo-generator-searchdb` 生成 JSON 索引，前端实现即时搜索，无需后端

## 实现说明

### 核心配置要点

- `_config.yml`：站点标题设为 ESP32/ESP8266 相关，URL 配置需与 GitHub Pages 仓库名一致（`https://<username>.github.io/<repo>/`）
- `hexo-deployer-git` 需配置正确的 `repo`、`branch` 字段指向 GitHub 仓库
- `_config.next.yml` 启用：本地搜索、RSS 链接、代码高亮、返回顶部、阅读进度条等

### 向后兼容

- 保留现有 `package.json` 的依赖和脚本不变
- 保留 `LICENSE` 和 `README.md` 文件

## 目录结构

```
microcontroller-blog/
├── _config.yml                      # [NEW] Hexo 主配置文件。站点元信息、URL、部署、目录结构、插件配置
├── _config.next.yml                 # [NEW] NexT 主题覆盖配置。主题样式、菜单、侧边栏、搜索、RSS、第三方集成
├── scaffolds/
│   ├── draft.md                     # [NEW] 草稿模板
│   ├── page.md                      # [NEW] 页面模板
│   └── post.md                      # [NEW] 文章模板（含 front-matter：title, date, tags, categories）
├── source/
│   ├── about/
│   │   └── index.md                 # [NEW] 关于页面
│   ├── categories/
│   │   └── index.md                 # [NEW] 分类归档页面
│   └── tags/
│       └── index.md                 # [NEW] 标签归档页面
├── themes/
│   └── next/                        # [NEW] NexT 主题（通过 Git Submodule 引入）
├── .gitmodules                      # [NEW] Git Submodule 配置
├── package.json                     # [MODIFY] 添加 hexo-theme-next 依赖说明
├── LICENSE                          # 保持不变
└── README.md                        # [MODIFY] 更新为博客说明文档
```

# Agent Extensions

No extensions needed for this task - this is a standard Hexo blog setup with manual file creation and Git operations.