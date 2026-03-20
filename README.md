# Microcontroller Blog

基于 [Hexo](https://hexo.io/) + [Butterfly](https://butterfly.js.org/) 主题搭建的单片机开发技术博客，专注于 ESP32/ESP8266 等 MCU 的开发笔记与教程分享。

## 功能特性

- 静态博客，部署到 GitHub Pages，访问速度快
- Butterfly 主题，界面简洁现代
- 支持 Markdown 编写文章，代码高亮
- 支持文章分类、标签、搜索
- 支持 Atom/RSS 订阅

## 技术栈

| 技术 | 说明 |
|------|------|
| [Hexo](https://hexo.io/) 8.1.1 | 静态站点生成器 |
| [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly) | 博客主题 |
| hexo-renderer-kramed | Markdown 渲染器 |
| hexo-renderer-pug / stylus | Butterfly 主题模板渲染 |
| hexo-deployer-git | Git 部署插件 |
| highlight.js | 代码语法高亮 |

## 项目结构

```
microcontroller-blog/
├── _config.yml              # Hexo 主配置
├── _config.butterfly.yml    # Butterfly 主题配置
├── package.json             # 依赖与脚本
├── scaffolds/               # 文章模板
│   ├── post.md              # 文章模板
│   ├── draft.md             # 草稿模板
│   └── page.md              # 页面模板
├── source/                  # 源文件
│   └── _posts/              # 博客文章（Markdown 文件）
└── themes/
    └── butterfly/           # Butterfly 主题文件
```

## 本地预览

严格按照以下链路操作：

```bash
# 1. 安装依赖
npm install

# 2. 清理缓存并重新生成静态文件
npx hexo clean && npx hexo generate

# 3. 启动本地开发服务器
npx hexo server
```

启动后访问 **http://localhost:4000/** 即可预览博客。

> **端口被占用时**，使用以下命令释放端口后重新启动：
> ```bash
> lsof -ti:4000 | xargs kill -9
> ```

也可以使用 `package.json` 中定义的快捷脚本：

```bash
npm start          # 等同于 hexo server
npm run build      # 等同于 hexo generate
npm run deploy     # 等同于 hexo deploy
```

## 部署到 GitHub Pages

项目配置了通过 `hexo-deployer-git` 自动部署到 `gh-pages` 分支。

部署前需先在 `_config.yml` 中配置 GitHub 认证信息：

```yaml
deploy:
  type: git
  repo: https://<TOKEN>@github.com/DK256/microcontroller-blog.git
  branch: gh-pages
```

> 将 `<TOKEN>` 替换为你的 GitHub Personal Access Token（需有 `repo` 权限）。**注意：不要将包含 Token 的配置提交到 Git 仓库，GitHub Push Protection 会拦截。**

执行部署：

```bash
npx hexo clean && npx hexo generate && npx hexo deploy
```

部署成功后，博客将发布到 GitHub Pages 的 `gh-pages` 分支。

## 添加新文章

### 使用命令创建

```bash
npx hexo new "文章标题"
```

这会在 `source/_posts/` 目录下生成一个 Markdown 文件。

### 文章 Front-Matter 格式

生成的文件包含以下 Front-Matter 头部信息，根据需要填写：

```yaml
---
title: 文章标题
date: 2026-03-20 00:00:00
updated: 2026-03-20 00:00:00
tags:
  - 标签1
  - 标签2
categories:
  - 分类名
description: 文章描述，用于 SEO 和文章摘要
---

正文内容使用 Markdown 编写...
```

### Front-Matter 字段说明

| 字段 | 必填 | 说明 |
|------|------|------|
| `title` | 是 | 文章标题 |
| `date` | 是 | 创建日期 |
| `updated` | 否 | 最后更新日期 |
| `tags` | 否 | 文章标签，支持多个 |
| `categories` | 否 | 文章分类，支持多级 |
| `description` | 否 | 文章描述，显示在文章列表摘要中 |

### 图片资源

项目已开启 `post_asset_folder: true`，每篇文章会自动创建同名文件夹用于存放图片资源。

例如文章为 `source/_posts/my-project.md`，则图片放在 `source/_posts/my-project/` 目录下，在文章中引用：

```markdown
![图片描述](my-project/image.png)
```

### 文章编写示例

参考已有的示例文章 `source/_posts/esp32-getting-started.md`，支持以下 Markdown 语法：

- 标题：`## 二级标题`、`### 三级标题`
- 代码块：使用 ` ```cpp ` 等指定语言
- 表格、列表、链接、加粗、图片等标准 Markdown 语法

### 创建后预览

```bash
npx hexo clean && npx hexo generate
npx hexo server
```

在浏览器中访问 http://localhost:4000/ 查看效果。

## 配置文件说明

- `_config.yml` — Hexo 主配置，包含站点信息、URL、部署、插件等
- `_config.butterfly.yml` — Butterfly 主题配置，包含导航栏、代码块样式、社交链接等

主题配置参考：https://butterfly.js.org/

## License

[MIT](LICENSE)
