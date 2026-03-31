# MkDocs 建站与使用指南

本文记录本网站从 0 到 1 的搭建过程，以及后续维护时常用的 MkDocs 工作流。

## 1. 项目结构说明

当前仓库核心结构如下：

```text
.
├─ .github/
│  └─ workflows/
│     └─ deploy.yml          # GitHub Actions 自动构建和部署
├─ docs/                     # 所有页面的 Markdown 源文件
│  ├─ index.md               # 主页
│  ├─ about.md               # 关于我
│  ├─ archive.md             # 归档
│  └─ coding/
│     └─ mkdocs-guide.md     # 本文档
├─ mkdocs.yml                # MkDocs 主配置
└─ requirements.txt          # Python 依赖
```

关键点：

1. `docs/` 是文档源目录，MkDocs 会把它转换为静态站点。
2. `mkdocs.yml` 负责站点标题、导航、主题、插件等配置。
3. `site/` 是构建产物目录（构建时生成，一般不提交）。

## 2. 依赖安装

建议先创建虚拟环境，再安装依赖。

```bash
python -m venv .venv
```

Windows PowerShell 激活：

```powershell
.\.venv\Scripts\Activate.ps1
```

安装依赖：

```bash
pip install -r requirements.txt
```

当前依赖包含：

1. `mkdocs`：静态文档站点生成器。
2. `mkdocs-material`：Material 主题，提供更完整的导航与搜索体验。

## 3. 本地开发流程

### 3.1 实时预览

```bash
mkdocs serve
```

默认本地地址：`http://127.0.0.1:8000`

每次修改 `docs/` 下 Markdown 或 `mkdocs.yml`，页面会自动刷新。

### 3.2 严格构建检查

```bash
mkdocs build --strict
```

`--strict` 很重要：

1. 导航引用了不存在的文档时会失败。
2. 链接或配置问题会在部署前暴露出来。

## 4. mkdocs.yml 常见配置

本站当前配置重点如下。

### 4.1 站点元信息

```yaml
site_name: Haohu 的个人网站
site_description: 记录编程、学习与生活的笔记
site_url: https://hhhydrogen.github.io/
```

用途：

1. 页面标题显示。
2. SEO 相关元信息。
3. 一些主题组件会读取 `site_url` 生成绝对链接。

### 4.2 主题设置

```yaml
theme:
  name: material
  language: zh
  features:
    - navigation.tabs
    - navigation.top
    - content.code.copy
```

解释：

1. `language: zh`：界面文案切换为中文。
2. `navigation.tabs`：顶部展示导航栏目。
3. `navigation.top`：滚动后显示返回顶部入口。
4. `content.code.copy`：代码块支持一键复制。

### 4.3 导航配置

```yaml
nav:
  - 主页: index.md
  - 关于我: about.md
  - 文章归档: archive.md
  - Coding:
      - MkDocs 建站与使用指南: coding/mkdocs-guide.md
```

规则：

1. 左侧是显示名称，右侧是 `docs/` 下的相对路径。
2. 支持分组（如 `Coding`）。
3. 配置顺序就是页面显示顺序。

### 4.4 插件和 Markdown 扩展

```yaml
plugins:
  - search

markdown_extensions:
  - admonition
  - toc:
      permalink: true
```

说明：

1. `search`：启用站内搜索。
2. `admonition`：支持提示块语法。
3. `toc`：为标题生成锚点，`permalink: true` 会显示可复制链接。

## 5. 新增页面的标准步骤

以新增一篇文章为例：

1. 在 `docs/` 新建 Markdown 文件，例如 `docs/coding/new-post.md`。
2. 在 `mkdocs.yml` 的 `nav` 加入条目。
3. 本地运行 `mkdocs build --strict`。
4. 提交代码并推送到 `main`。
5. 等待 GitHub Actions 部署完成后访问线上页面。

## 6. GitHub Pages 自动部署流程

自动部署由 `.github/workflows/deploy.yml` 完成，触发条件是推送到 `main` 分支。

流程是：

1. 拉取代码。
2. 安装 Python 和依赖。
3. 执行 `mkdocs build --strict`。
4. 上传 `site/` 构建产物。
5. 发布到 GitHub Pages。

前提条件：

1. 仓库 `Settings -> Pages -> Source` 设为 `GitHub Actions`。
2. 仓库默认分支为 `main`（与工作流触发条件一致）。

## 7. 常见问题排查

### 7.1 看不到导航栏目

检查顺序：

1. `mkdocs.yml` 是否配置了多个 `nav` 项。
2. 对应 Markdown 文件是否真实存在于 `docs/`。
3. GitHub Pages Source 是否为 `GitHub Actions`。
4. 浏览器是否命中缓存（可强刷 `Ctrl + F5`）。

### 7.2 本地构建报主题不存在

典型报错：`Unrecognised theme name: 'material'`

处理方式：

1. 重新安装依赖：`pip install -r requirements.txt`
2. 确认当前终端处于正确虚拟环境。

### 7.3 页面是 404

常见原因：

1. 页面文件已创建，但没有加入 `nav`。
2. 工作流还没跑完。
3. 访问路径和配置路径不一致。

## 8. 后续可扩展方向

1. 引入博客插件，支持按时间线管理文章。
2. 增加分类与标签页面。
3. 自定义主题配色与字体，形成个人风格。
4. 增加评论系统或访问统计。

---

如果后续你调整了目录结构，记得同步更新本指南，避免配置和文档说明不一致。
