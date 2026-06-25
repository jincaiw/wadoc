# 配置说明

## MkDocs 配置

本站的完整配置位于 `mkdocs.yml`。

### 主题

使用 Material for MkDocs 主题，支持：

- 深色/浅色模式切换
- 中文界面
- 响应式布局
- 代码高亮与复制
- 站内搜索（中英文）

### 插件

| 插件 | 功能 |
|------|------|
| `search` | 站内全文搜索，支持中文分词 |
| `tags` | 标签系统，方便内容分类 |

### Markdown 扩展

| 扩展 | 功能 |
|------|------|
| `admonition` | 提示块（note, tip, warning 等） |
| `attr_list` | 属性列表，允许添加 CSS 类 |
| `footnotes` | 脚注支持 |
| `toc` | 目录生成，自动锚点 |
| `pymdownx.highlight` | 代码语法高亮 |
| `pymdownx.superfences` | 代码块嵌套支持 |
| `pymdownx.tabbed` | 选项卡内容 |
| `pymdownx.tasklist` | 任务列表 |
| `pymdownx.emoji` | Emoji 表情支持 |

## 自定义样式

自定义样式位于 `docs/stylesheets/extra.css`。

可以修改：

- 字体和排版
- 颜色方案
- 间距和布局
- 组件样式

## CI/CD 配置

部署配置位于 `.github/workflows/deploy-docs.yml`。

触发方式：

1. 推送到默认分支（自动构建并部署）
2. 创建 Pull Request（仅构建验证）
3. 手动触发（GitHub Actions 页面的 "Run workflow"）

工作流步骤：

1. 检出代码
2. 设置 Python
3. 安装依赖
4. 严格模式构建
5. 上传构建产物
6. 部署到 GitHub Pages
