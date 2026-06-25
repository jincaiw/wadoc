# API 参考

## 概述

本文档提供本站使用的主要接口和配置参考。

## 构建 API

### `mkdocs build`

构建静态站点。

```bash
mkdocs build [--strict] [--clean]
```

参数：

| 参数 | 说明 |
|------|------|
| `--strict` | 严格模式，将警告视为错误 |
| `--clean` | 构建前清理 site 目录 |

### `mkdocs serve`

启动本地开发服务器。

```bash
mkdocs serve [--dev-addr ADDR] [--livereload]
```

参数：

| 参数 | 说明 |
|------|------|
| `--dev-addr` | 指定监听地址，默认 `127.0.0.1:8000` |
| `--livereload` | 启用自动重载（默认启用） |

## 配置参考

### 站点配置

| 字段 | 类型 | 说明 |
|------|------|------|
| `site_name` | string | 站点名称 |
| `site_description` | string | 站点描述 |
| `site_url` | string | 站点 URL |
| `repo_url` | string | 仓库地址 |
| `edit_uri` | string | 编辑链接路径 |

### 主题配置

!!! note "Material for MkDocs"
    本站使用 Material 主题，详细的主题配置请参考
    [官方文档](https://squidfunk.github.io/mkdocs-material/setup/)。

## 部署 API

本站通过 GitHub Actions 自动部署到 GitHub Pages。

工作流文件位于 `.github/workflows/deploy-docs.yml`。

- 推送到 `main` 分支时自动构建并部署
- Pull Request 仅构建不做部署
- 支持手动触发（workflow_dispatch）
