---
title: "Hugo 博客上手指南：主题配置 & GitHub Pages 部署 & Git 命令总结"
date: 2026-04-04T23:00:00+08:00
draft: false
description: "一篇涵盖 Hugo 主题更改语法、GitHub Pages 部署步骤、常用 Git 命令的完整上手文档"
tags: ["Engineering", "Introductions", "Mathematics"]
categories: ["Projects","Skill Acquisition","Life"]
---

## 一、认识 Hugo 博客的目录结构

```text
my_blog/
├── hugo.toml              # 站点主配置文件
├── content/               # 所有文章内容
│   └── post/              # 文章目录
│       └── hello_my_blog.md
├── static/                # 静态资源（图片、favicon 等）
├── assets/                # 样式、脚本等资源
├── layouts/               # 自定义模板（覆盖主题模板）
├── themes/                # 主题目录
│   └── hugo-theme-stack-master/
├── public/                # hugo 构建后生成的静态网页
├── archetypes/            # 新文章模板
├── data/                  # 数据文件
└── i18n/                  # 多语言翻译文件
```

**核心规律：**
- 改配置 → `hugo.toml`
- 写文章 → `content/post/`
- 放图片 → `static/`
- 改外观 → `hugo.toml` + `layouts/` + `assets/`

---

## 二、主题更改的详细语法

### 2.1 hugo.toml 基础字段

当前配置文件内容：

```toml
baseURL = 'https://example.org/'
locale = 'en-us'
title = 'Blog For Life'
theme = "hugo-theme-stack-master"
```

各字段含义：

| 字段 | 作用 | 修改建议 |
|------|------|----------|
| `baseURL` | 网站最终发布的网址 | 部署到 GitHub Pages 后改成实际地址 |
| `locale` | 语言地区 | 中文博客改成 `'zh-cn'` |
| `title` | 网站标题，显示在首页和浏览器标签 | 改成你的博客名 |
| `theme` | 使用的主题目录名 | 必须和 `themes/` 下的文件夹名一致 |

### 2.2 TOML 语法基础

Hugo 配置文件使用 TOML 格式，常用语法：

```toml
# 字符串
title = "我的博客"

# 布尔值
draft = false

# 数字
paginate = 10

# 数组
tags = ["Hugo", "博客", "教程"]

# 表（类似对象/字典）
[params]
  description = "一个个人博客"
  mainSections = ["post"]

# 嵌套表
[params.sidebar]
  emoji = "🎯"
  subtitle = "记录生活与技术"

# 双括号表示数组中的表
[[menu.main]]
  name = "首页"
  url = "/"
  weight = 1

[[menu.main]]
  name = "归档"
  url = "/archives"
  weight = 2
```

### 2.3 修改站点标题和描述

```toml
title = 'Blog For Life'

[params]
  description = "记录我的学习与生活"
```

效果：`title` 显示在浏览器标签页和网站顶部，`description` 显示在首页副标题或 SEO 描述中。

### 2.4 修改语言

```toml
languageCode = 'zh-cn'
defaultContentLanguage = 'zh-cn'
```

### 2.5 配置首页侧边栏（Stack 主题）

```toml
[params.sidebar]
  emoji = "🏠"
  subtitle = "欢迎来到我的博客"

[params.sidebar.avatar]
  enabled = true
  local = true
  src = "img/avatar.png"   # 图片放在 static/img/avatar.png
```

把头像图片放在：`static/img/avatar.png`

### 2.6 配置社交链接（Stack 主题）

```toml
[[params.widgets.social]]
  name = "GitHub"
  icon = "brand-github"
  url = "https://github.com/你的用户名"

[[params.widgets.social]]
  name = "Email"
  icon = "mail"
  url = "mailto:你的邮箱@example.com"
```

### 2.7 配置导航菜单

```toml
[[menu.main]]
  name = "首页"
  url = "/"
  weight = 1

[[menu.main]]
  name = "归档"
  url = "/archives"
  weight = 2

[[menu.main]]
  name = "搜索"
  url = "/search"
  weight = 3
```

`weight` 数字越小排越前面。

### 2.8 配置文章每页显示数量

```toml
paginate = 5
```

### 2.9 修改 favicon

把你的 favicon 文件放到：

```
static/favicon.ico
```

或者：

```
static/favicon.png
```

Hugo 会自动识别。

### 2.10 文章 Front Matter 语法

每篇文章开头的 `---` 之间的内容叫 Front Matter，控制这篇文章的元信息：

```markdown
---
title: "文章标题"
date: 2026-04-04T23:00:00+08:00
draft: false
description: "文章描述，用于 SEO 和列表页预览"
tags: ["标签1", "标签2"]
categories: ["分类名"]
image: "cover.jpg"
---

正文从这里开始...
```

| 字段 | 说明 |
|------|------|
| `title` | 文章标题 |
| `date` | 发布日期，影响排序 |
| `draft` | `true` 表示草稿，不会发布；`false` 才会显示 |
| `description` | 文章描述 |
| `tags` | 标签，数组格式 |
| `categories` | 分类 |
| `image` | 文章封面图（Stack 主题支持） |

### 2.11 在文章中插入图片

把图片放到 `static/images/` 目录：

```
static/images/my-photo.png
```

文章中引用：

```markdown
![图片说明](/images/my-photo.png)
```

### 2.12 "改哪里影响哪里"速查表

| 想改什么 | 改哪个文件 |
|----------|------------|
| 博客标题 | `hugo.toml` → `title` |
| 网站地址 | `hugo.toml` → `baseURL` |
| 语言 | `hugo.toml` → `languageCode` |
| 头像 | `static/img/avatar.png` + `hugo.toml` 配置 |
| 导航菜单 | `hugo.toml` → `[[menu.main]]` |
| 社交链接 | `hugo.toml` → `[[params.widgets.social]]` |
| 新文章 | `content/post/新文章.md` |
| 文章封面图 | Front Matter 的 `image` 字段 |
| 网站图标 | `static/favicon.ico` |
| 页面模板 | `layouts/` 覆盖主题模板 |
| 自定义 CSS | `assets/scss/custom.scss`（Stack 主题支持） |

---

## 三、部署到 GitHub Pages 的详细步骤

### 3.1 前提条件

- 已安装 Git（命令行输入 `git --version` 确认）
- 已有 GitHub 账号
- 已安装 Hugo Extended（命令行输入 `hugo version` 确认）

### 3.2 两种 GitHub Pages 方式

| 方式 | 仓库名 | 最终网址 |
|------|--------|----------|
| 用户站点 | `用户名.github.io` | `https://用户名.github.io/` |
| 项目站点 | 任意名字，如 `my_blog` | `https://用户名.github.io/my_blog/` |

推荐新手用 **用户站点**，网址更简洁。

### 3.3 详细操作步骤

#### 第一步：在 GitHub 创建仓库

1. 登录 [GitHub](https://github.com)
2. 点右上角 **+** → **New repository**
3. 仓库名填写：
   - 用户站点：`你的用户名.github.io`
   - 项目站点：`my_blog`（或任意名字）
4. 选择 **Public**
5. **不要**勾选 "Add a README file"
6. 点 **Create repository**

#### 第二步：修改 hugo.toml 中的 baseURL

**用户站点：**

```toml
baseURL = 'https://你的用户名.github.io/'
```

**项目站点：**

```toml
baseURL = 'https://你的用户名.github.io/my_blog/'
```

> ⚠️ 这一步非常重要！`baseURL` 写错会导致部署后样式丢失或页面 404。

#### 第三步：构建静态文件

在博客根目录执行：

```bash
hugo
```

这会在 `public/` 目录生成完整的静态网站文件。

#### 第四步：初始化 Git 仓库并推送

**方法 A：只推送 public 目录（简单方式）**

```bash
cd public
git init
git add .
git commit -m "first deploy"
git branch -M main
git remote add origin https://github.com/你的用户名/你的仓库名.git
git push -u origin main
```

然后在 GitHub 仓库 → Settings → Pages → Source 选择 `main` 分支，目录选 `/ (root)`。

**方法 B：推送整个项目 + GitHub Actions 自动构建（推荐）**

```bash
cd c:\Users\minfu\my_blog
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/你的用户名/你的仓库名.git
git push -u origin main
```

然后在仓库中创建 GitHub Actions 工作流文件，让 GitHub 自动帮你构建和部署。

#### 第五步：配置 GitHub Actions 自动部署（推荐方法 B 时使用）

在项目根目录创建文件：`.github/workflows/hugo.yml`

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.147.6
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb

      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo --minify --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

推送后，在 GitHub 仓库 → **Settings** → **Pages** → **Source** 选择 **GitHub Actions**。

#### 第六步：访问你的博客

等 GitHub Actions 构建完成（通常 1-2 分钟），访问：

- 用户站点：`https://你的用户名.github.io/`
- 项目站点：`https://你的用户名.github.io/仓库名/`

### 3.4 常见问题排查

| 问题 | 原因 | 解决方法 |
|------|------|----------|
| 页面 404 | `baseURL` 写错或 Pages 源未配置 | 检查 `baseURL`，确认 Pages 设置 |
| 样式全部丢失 | `baseURL` 路径不对 | 项目站点必须包含仓库名路径 |
| 文章不显示 | `draft: true` | 改成 `draft: false` |
| 本地正常但线上不对 | 本地用 `hugo server` 会忽略 `baseURL` | 用 `hugo` 构建后检查 `public/` |

### 3.5 后续更新文章的流程

每次写完新文章后：

```bash
# 1. 添加所有改动
git add .

# 2. 提交
git commit -m "新增文章：文章标题"

# 3. 推送到 GitHub（如果用了 Actions，会自动重新部署）
git push
```

---

## 四、常用 Git 命令总结

### 4.1 仓库初始化与配置

```bash
# 初始化一个新的 Git 仓库
git init

# 设置用户名（首次使用 Git 时需要）
git config --global user.name "你的名字"

# 设置邮箱
git config --global user.email "你的邮箱@example.com"

# 查看当前配置
git config --list
```

### 4.2 基本操作

```bash
# 查看仓库状态（哪些文件被修改、哪些未跟踪）
git status

# 添加单个文件到暂存区
git add 文件名

# 添加所有文件到暂存区
git add .

# 提交暂存区的文件（附带说明信息）
git commit -m "提交说明"

# 查看提交历史（简洁模式）
git log --oneline

# 查看提交历史（详细模式）
git log
```

### 4.3 远程仓库操作

```bash
# 关联远程仓库
git remote add origin https://github.com/用户名/仓库名.git

# 查看已关联的远程仓库
git remote -v

# 第一次推送到远程（设置默认上游分支）
git push -u origin main

# 后续推送
git push

# 从远程拉取最新代码
git pull

# 克隆一个已有的远程仓库到本地
git clone https://github.com/用户名/仓库名.git
```

### 4.4 分支操作

```bash
# 查看所有分支
git branch

# 创建新分支
git branch 分支名

# 切换到某个分支
git checkout 分支名

# 创建并切换到新分支
git checkout -b 新分支名

# 将当前分支重命名为 main
git branch -M main

# 合并某个分支到当前分支
git merge 分支名
```

### 4.5 查看差异

```bash
# 查看工作区和暂存区的差异
git diff

# 查看暂存区和最近一次提交的差异
git diff --cached

# 查看某个文件的修改
git diff 文件名
```

### 4.6 撤销操作

```bash
# 撤销工作区某个文件的修改（恢复到最近提交的状态）
git checkout -- 文件名

# 将文件从暂存区移回工作区（不删除修改）
git reset HEAD 文件名

# 回退到上一次提交（保留文件修改）
git reset --soft HEAD~1

# 回退到上一次提交（丢弃所有修改，⚠️ 危险操作）
git reset --hard HEAD~1
```

### 4.7 .gitignore 文件

在项目根目录创建 `.gitignore`，写入不需要提交的文件：

```text
# Hugo 构建输出（如果用 GitHub Actions 构建则不需要提交）
public/

# Hugo 资源缓存
resources/

# 系统文件
.DS_Store
Thumbs.db

# 编辑器文件
.vscode/
*.swp
```

### 4.8 日常工作流总结

```bash
# 典型的"写文章 → 发布"流程：

# 1. 写文章
#    在 content/post/ 下新建 .md 文件

# 2. 本地预览
hugo server

# 3. 确认没问题后，添加改动
git add .

# 4. 提交
git commit -m "新增文章：xxx"

# 5. 推送到 GitHub（自动触发部署）
git push
```

---

## 五、总结

| 要做的事 | 操作 |
|----------|------|
| 改博客外观 | 编辑 `hugo.toml` |
| 写新文章 | 在 `content/post/` 下新建 `.md` 文件 |
| 本地预览 | 运行 `hugo server` |
| 部署上线 | `git add . → git commit → git push` |
| 查看网站 | 访问 `https://你的用户名.github.io/` |