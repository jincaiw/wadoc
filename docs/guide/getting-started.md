# 快速开始

欢迎使用王记文档。本指南将帮助你快速上手。

## 环境准备

确保你的环境中已安装以下工具：

- Python 3.8+
- pip（Python 包管理器）
- Git

## 安装依赖

```bash
pip install -r requirements-docs.txt
```

## 本地预览

```bash
mkdocs serve
```

在浏览器中打开 `http://127.0.0.1:8000` 即可预览。

## 构建静态站点

```bash
mkdocs build --strict
```

构建产物输出到 `site/` 目录，可直接部署到任何静态托管服务。

## 目录结构

```
.
├── docs/               # 文档源文件
│   ├── index.md        # 首页
│   ├── assets/         # 静态资源（图片等）
│   ├── stylesheets/    # 样式文件
│   ├── guide/          # 指南
│   ├── reference/      # 参考文档
│   └── about/          # 关于
├── mkdocs.yml          # MkDocs 配置
├── requirements-docs.txt
└── .github/workflows/  # CI/CD 配置
```
