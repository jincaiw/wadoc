# 从零搭建 Material for MkDocs 文档站

本文记录从零开始搭建本站的过程，涵盖环境准备、配置、文档整理、CI/CD、自定义域名等完整流程。

## 为什么选择 Material for MkDocs

市面上有很多文档构建工具，最终选择 Material for MkDocs 的原因：

| 需求 | 方案 |
|------|------|
| Markdown 写作 | 纯文本，专注内容，版本友好 |
| 中文支持 | 原生中文界面，支持中文搜索分词 |
| 美观度 | Material Design，开箱即用 |
| 响应式 | 桌面、平板、手机自适应 |
| 深色模式 | 自带浅色/深色切换 |
| 可扩展 | 丰富的插件和 Markdown 扩展生态 |
| 部署 | 生成纯静态 HTML，可部署到任何平台 |
| 免费 | 开源免费，无付费功能 |

## 第一步：项目初始化

### 创建项目结构

```bash
mkdir wadoc && cd wadoc
```

项目根目录下创建如下结构：

```
.
├── docs/                  # 文档源目录
│   ├── index.md           # 首页
│   ├── assets/            # 静态资源
│   ├── stylesheets/       # 自定义样式
│   ├── guide/             # 指南
│   ├── mac/               # Mac 教程
│   ├── docker/            # Docker 文档
│   ├── linux/             # Linux 学习
│   ├── reference/         # 参考
│   └── about/             # 关于
├── mkdocs.yml             # 主配置
├── requirements-docs.txt  # Python 依赖
└── .github/workflows/     # CI/CD 配置
```

### 依赖文件

`requirements-docs.txt` 只需一行：

```txt
mkdocs-material==9.*
```

## 第二步：核心配置

### 站点配置

```yaml
# mkdocs.yml
site_name: jason.wa 的文档库
site_description: 个人知识库与技术文档
site_url: https://docx.mujizi.com
repo_url: https://github.com/jincaiw/wadoc
edit_uri: edit/main/docs/
```

### 主题

Material 主题提供了丰富的功能开关：

```yaml
theme:
  name: material
  language: zh           # 中文界面
  font: false            # 使用系统字体
  palette:
    - scheme: default    # 浅色模式
      toggle:
        icon: material/brightness-7
        name: 切换到深色模式
    - scheme: slate      # 深色模式
      toggle:
        icon: material/brightness-4
        name: 切换到浅色模式
  features:
    - navigation.sections    # 分组导航
    - navigation.top         # 回到顶部
    - navigation.footer      # 页脚导航
    - toc.follow             # 目录跟随
    - search.suggest         # 搜索建议
    - search.highlight       # 搜索高亮
    - content.code.copy      # 代码复制
    - content.action.edit    # 编辑链接
```

### Markdown 扩展

搭建过程中踩过一个坑：**首页卡片不渲染**。

问题在于 `<div class="grid cards" markdown>` 内部的 Markdown 需要 `md_in_html` 扩展来解析，但最初只配置了 `attr_list`。排查过程：

1. 发现构建后 HTML 输出的是原始 Markdown 而非渲染结果
2. 通过 Python 测试确认 `attr_list` 不包含 `md_in_html` 功能
3. 添加 `md_in_html` 扩展后修复

最终启用的扩展：

```yaml
markdown_extensions:
  - admonition       # 提示块
  - attr_list        # HTML 属性
  - md_in_html       # ★ HTML 内 Markdown 解析（关键）
  - footnotes        # 脚注
  - tables           # 表格
  - toc              # 目录锚点
  - pymdownx.highlight     # 代码高亮
  - pymdownx.superfences   # 代码块嵌套
  - pymdownx.tabbed        # 选项卡
  - pymdownx.tasklist      # 任务列表
  - pymdownx.emoji         # Emoji
```

### 插件

```yaml
plugins:
  - search:
      lang:
        - zh
        - en
  - tags
```

搜索插件配置了中英文双语言支持，tags 插件提供标签分类。

## 第三步：文档整理

### 导航设计

原始文档来自多个目录，需要合理的导航归类：

```
首页
├── 指南              # 站点使用说明
├── 图床搭建           # 独立主题
├── Mac 教程           # → 4 个子分组
│   ├── 系统设置
│   ├── 软件安装
│   ├── 开发环境
│   └── 其他
├── Docker            # Docker 部署
├── Linux             # Linux 学习
├── 参考              # API 和配置
└── 关于              # 项目信息
```

### 文档审阅要点

将外部 Markdown 文件引入项目时，需要检查：

1. **标题层级**：每页只保留一个 `#` 标题
2. **代码块语言**：补全语言标识以获得语法高亮
3. **图片路径**：本地路径（如 Typora 的 `/Users/...`）需替换为可访问的 URL
4. **CSDN 标记**：移除 `#pic_center` 等不兼容后缀
5. **残缺语法**：如嵌套的 `![![img](a.png](b.png)` 需要修正
6. **空章节**：补全或移除

### 导航过长怎么办

Mac 教程 14 篇文章如果平铺在侧边栏会非常拥挤。解决方式：使用二级折叠分组。

```yaml
nav:
  - Mac 教程:
      - 概述: mac/index.md
      - 系统设置:
          - 初始化配置指南: mac/init-config.md
          - 关闭 SIP: mac/disable-sip.md
          - ...
```

这样侧边栏默认只显示 5 个顶级条目（概述 + 4 个分组），点击展开查看具体文章。

## 第四步：样式优化

### 中文排版

```css
/* 系统字体栈 */
body {
  font-family: -apple-system, BlinkMacSystemFont, "PingFang SC",
    "Hiragino Sans GB", "Microsoft YaHei", "Noto Sans CJK SC",
    "Helvetica Neue", Helvetica, Arial, sans-serif;
}

/* 正文行高 */
.md-content p, .md-content li {
  line-height: 1.8;
}

/* 正文宽度约束 */
.md-content {
  max-width: 48rem;
}
```

### 首页卡片

为 4 列网格添加微交互：

```css
.md-typeset .grid.cards > ul > li:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}
```

深色模式也要适配：

```css
[data-md-color-scheme="slate"] .md-typeset .grid.cards > ul > li:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.35);
}
```

## 第五步：CI/CD 部署

### GitHub Actions

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
        with:
          python-version: "3.12"
          cache: pip
      - run: pip install -r requirements-docs.txt
      - run: mkdocs build --strict    # 严格模式
      - uses: actions/upload-pages-artifact@v3
        with:
          path: site

  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

关键点：

- `push` 到默认分支：构建 + 部署
- `pull_request`：只构建不部署（验证 PR 不破坏构建）
- `workflow_dispatch`：支持手动触发
- `--strict`：将警告视为错误，保证构建质量

### 启用 GitHub Pages

第一次部署前，需要在 GitHub 仓库设置中启用 Pages：

```
Settings → Pages → Source: GitHub Actions
```

也可以通过 API 启用：

```bash
echo '{"build_type":"workflow","source":{"branch":"main","path":"/"}}' \
  | gh api repos/owner/repo/pages -X POST --input -
```

## 第六步：自定义域名

### CNAME 文件

在 `docs/` 目录下创建 `CNAME` 文件：

```
docx.mujizi.com
```

### DNS 配置

在域名 DNS 管理中添加 CNAME 记录：

```
docx.mujizi.com → jincaiw.github.io
```

### 通过 API 设置

```bash
echo '{"cname":"docx.mujizi.com"}' \
  | gh api repos/owner/repo/pages -X PUT --input -
```

设置后 GitHub 会自动申请 SSL 证书，等待几分钟即可通过 HTTPS 访问。

## 构建验证

每次修改后本地验证：

```bash
python3 -m venv .venv-docs
source .venv-docs/bin/activate
pip install -r requirements-docs.txt
mkdocs build --strict
```

构建产物在 `site/` 目录，可直接预览。

## 经验总结

### 遇到的坑

1. **`md_in_html` 缺失**：卡片内的 Markdown 不渲染。罪魁祸首是只配置了 `attr_list`，没有开启 `md_in_html` 扩展。
2. **本地图片路径**：从 Typora 复制的文档包含 `/Users/xxx/Library/...` 的本地路径，需要替换为可公开访问的图片 URL。
3. **YAML 特殊字符**：中文引号 `""` 会导致 YAML 解析错误，需要用普通引号包裹或去掉。
4. **CSDN 图片标记**：`#pic_center` 后缀在 MkDocs 中不兼容，需要移除。
5. **导航条目过多**：14 个文章平铺在侧边栏过于拥挤，需要分组折叠。

### 最佳实践

- 每页只用一个 `#` 标题
- 代码块始终指定语言
- 站内链接使用相对路径
- 中英文混排加空格
- 定期 `mkdocs build --strict` 验证
- `.venv-docs/` 和 `site/` 加入 `.gitignore`
