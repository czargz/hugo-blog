# Hugo 静态博客（VS Code + Front Matter CMS + GitHub Pages）

本仓库已按「月光博客」方案完成初始化，编译与发布全部由 GitHub Actions 自动完成，本机只负责写作。

## 目录结构

| 路径 | 作用 |
|---|---|
| `hugo.toml` | 站点主配置（baseURL、标题、主题、菜单） |
| `content/posts/` | 文章正文，Front Matter CMS 的写作目录 |
| `content/search.md` | 搜索页（PaperMod 内置 search 布局） |
| `content/archives.md` | 归档页（PaperMod 内置 archives 布局） |
| `static/images/` | 图片素材，Front Matter 拖拽入库的位置 |
| `archetypes/default.md` | 新建文章的 front matter 模板 |
| `frontmatter.json` | Front Matter CMS 插件配置 |
| `.github/workflows/hugo.yaml` | 自动构建部署工作流（Hugo 官方当前模板） |
| `.tools/hugo/hugo.exe` | 本地 Hugo v0.168.0 **extended**（工作区上级目录） |

## 已预置内容

- Hugo v0.168.0 extended（主题 PaperMod 依赖 SCSS/PostCSS，非 extended 版无法编译）
- PaperMod 主题，以 Git submodule 方式挂载在 `themes/PaperMod`
- 官方 Actions 工作流：checkout 带 `submodules: recursive` + `fetch-depth: 0`，并安装 Dart Sass 与 Go/Node
- 中文站点配置：中文字数统计、阅读时间、归档与搜索入口

## 你还需要提供的素材

### 必需

| 素材 | 说明 | 给我什么 |
|---|---|---|
| GitHub 用户名 + 仓库名 | 决定 `baseURL` 与访问地址 | 例如 `bill / my-blog` |
| 站点标题与描述 | 显示在首页与浏览器标签 | 一句话即可 |
| 作者名 | 页脚与文章署名 | Bill 或想用的笔名 |
| 文章内容 | 正文主体 | 任选：Markdown / Word / PDF / 网页链接 / 纯文本，我来转换与排版 |
| 图片素材 | 配图、封面、截图 | 文件本体，或本机路径；我会统一落到 `static/images/` |

### 可选

| 素材 | 说明 |
|---|---|
| 自定义域名 | 绑定后我写 `static/CNAME`，并把 `baseURL` 改成域名 |
| 头像 / Logo | 首页 profile 模式使用 |
| 社交链接 | GitHub、X、邮箱、RSS |
| 评论系统 | Giscus / utterances（需公开仓库） |
| 访问统计 | Umami / GA / 百度统计 |
| 显示偏好 | 深浅色、目录、阅读时间、分享按钮、封面图 |

## 日常使用流程

1. 本地预览：`./.tools/hugo/hugo.exe server -D`（工作区根目录执行）
2. 写文章：VS Code → Front Matter 面板 → 创建内容 → 目录选 `content/posts`
3. 发布：把 `draft: true` 改成 `draft: false`
4. 提交推送：VS Code 源代码管理 → 提交 → 推送
5. 等待 Actions 构建完成（约数十秒），线上自动更新

## 已知坑位（已在本仓库规避）

1. **必须用 Hugo extended**：普通版编译 PaperMod 会因缺少 Sass 报错。
2. **Dart Sass 需单独安装**：新版 Hugo 不再内置，工作流里已加安装步骤。
3. **`fetch-depth: 0`**：否则 `.Lastmod` 与 `.GitInfo` 取不到，文章修改时间会错。
4. **`baseURL` 要带子路径**：项目页是 `https://用户名.github.io/仓库名/`，漏掉子路径会导致 CSS/图片全部 404。
5. **`.frontmatter/database/` 要提交**：内含图片元数据（`mediaDb.json`），删了会丢失图片标题与 alt 信息。
