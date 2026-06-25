# 从零搭建 Material for MkDocs 文档站

本文记录从零搭建本站的过程和踩坑记录。

## 选型

选择 Material for MkDocs 的理由：纯 Markdown 写作、原生中文支持、开箱即用的 Material Design、响应式布局、深色模式、丰富插件生态、生成纯静态 HTML 可部署到任何平台，且完全免费。

## 项目结构

```
.
├── docs/
│   ├── index.md           # 首页
│   ├── assets/            # SVG 图标
│   ├── stylesheets/       # 自定义样式
│   ├── guide/             # 指南
│   ├── mac/               # Mac 教程（13 篇）
│   ├── docker/            # Docker 部署
│   ├── linux/             # Linux 学习
│   ├── reference/         # 参考
│   └── about/             # 关于
├── mkdocs.yml
└── .github/workflows/
```

依赖文件仅一行：`mkdocs-material==9.*`。

## 配置要点

### 主题

中文界面、系统字体、一键深色/浅色切换。关键 feature 开关：

```yaml
features:
  - navigation.sections    # 分组导航
  - navigation.top         # 回到顶部
  - toc.follow             # 目录跟随
  - search.suggest         # 搜索建议
  - content.code.copy      # 代码复制按钮
  - content.action.edit    # GitHub 编辑入口
```

### Markdown 扩展

```yaml
markdown_extensions:
  - admonition       # 提示块
  - attr_list        # HTML 属性
  - md_in_html       # ★ HTML 内 Markdown 解析
  - pymdownx.highlight     # 代码高亮
  - pymdownx.superfences   # 代码块嵌套
  - pymdownx.tabbed        # 选项卡
  - pymdownx.tasklist      # 任务列表
  - pymdownx.emoji         # Emoji
```

### 搜索

```yaml
plugins:
  - search:
      lang: [zh, en]
  - tags
```

## 文档整理

### 导航分组

Mac 教程有 14 篇文章，平铺在侧边栏会非常拥挤。使用二级折叠分组：

```yaml
nav:
  - Mac 教程:
      - 概述: mac/index.md
      - 系统设置:
          - 初始化配置指南: mac/init-config.md
          - 关闭 SIP: mac/disable-sip.md
          - ...
```

侧边栏默认只显示顶级分组，点击展开查看子文章。

### 外部文档审阅

引入外部 Markdown 文件时，重点检查：

1. **标题层级**：每页只保留一个 `#` 标题
2. **代码块语言**：补全标识以获得语法高亮
3. **本地图片路径**：Typora 导出的 `/Users/xxx/...` 路径需替换为公开 URL
4. **CSDN 标记**：移除 `#pic_center` 等不兼容后缀
5. **残缺语法**：如嵌套 `![![img](a.png](b.png)` 需修正

## 样式优化

中文系统字体栈和排版优化：

```css
body {
  font-family: -apple-system, BlinkMacSystemFont, "PingFang SC",
    "Hiragino Sans GB", "Microsoft YaHei", "Noto Sans CJK SC",
    "Helvetica Neue", Helvetica, Arial, sans-serif;
}
.md-content p, .md-content li { line-height: 1.8; }
.md-content { max-width: 48rem; }
```

首页卡片添加微交互：

```css
.md-typeset .grid.cards > ul > li:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}
```

## CI/CD 部署

GitHub Actions 配置要点：

- `push` 到 `main`：自动构建并部署
- `pull_request`：仅构建验证，不部署
- `workflow_dispatch`：支持手动触发
- `mkdocs build --strict`：严格模式，警告即错误

完整工作流：

```yaml
name: Deploy Docs
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: "3.12", cache: pip }
      - run: pip install -r requirements-docs.txt
      - run: mkdocs build --strict
      - uses: actions/upload-pages-artifact@v3
        with: { path: site }
  deploy:
    needs: build
    environment: { name: github-pages }
    steps:
      - uses: actions/deploy-pages@v4
```

首次部署前，需要在仓库设置或通过 API 启用 Pages：

```bash
echo '{"build_type":"workflow","source":{"branch":"main","path":"/"}}' \
  | gh api repos/owner/repo/pages -X POST --input -
```

自定义域名只需在 `docs/CNAME` 写入域名，DNS 添加 CNAME 记录即可。

## 踩坑记录

### 1. 首页卡片不渲染

`<div class="grid cards" markdown>` 内部的 Markdown 需要 `md_in_html` 扩展来解析。最初只配置了 `attr_list`，发现构建输出的是原始 Markdown 文本而非渲染后的列表。添加 `md_in_html` 后修复。

### 2. YAML 中文引号报错

导航条目 `"已损坏，打不开的解决方法"` 中的中文引号 `""` 导致 YAML 解析失败。去掉引号改用纯文本即可。

### 3. 导航条目过多

Mac 教程 14 篇平铺让侧边栏超过一屏。改用二级折叠分组后，默认只显示 5 个顶级条目，视觉清爽许多。

### 4. 外来文档的图片

从 Typora 同步的文档含有 `/Users/jincai/Library/...` 本地路径，无法在线上访问。凡是本地路径的图片都需要上传到图床或替换为公开 URL。

### 5. CSDN 图片后缀不兼容

从 CSDN 导出的图片带有 `#pic_center` 后缀，MkDocs 不识别此标记，渲染为纯文本。批量移除即可。

## 本地验证

```bash
python3 -m venv .venv-docs
source .venv-docs/bin/activate
pip install -r requirements-docs.txt
mkdocs build --strict
```

将 `.venv-docs/` 和 `site/` 加入 `.gitignore`。
