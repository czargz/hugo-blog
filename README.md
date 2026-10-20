# Hugo 静态博客（VS Code + Front Matter CMS + GitHub Pages）

本仓库已按「月光博客」方案完成初始化，编译与发布全部由 GitHub Actions 自动完成，本机只负责写作。

**线上地址：https://czargz.github.io/hugo-blog/**（2026-10-20 已上线验证，HTTP 200）

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
| `themes/PaperMod` | PaperMod 主题，Git submodule 挂载 |
| `.tools/hugo/hugo.exe` | 本地 Hugo v0.168.0 **extended**（工作区上级目录，未全局安装） |

## 已预置内容

- Hugo v0.168.0 extended（主题 PaperMod 依赖 SCSS/PostCSS，非 extended 版无法编译）
- PaperMod 主题，以 Git submodule 方式挂载在 `themes/PaperMod`
- 官方 Actions 工作流：checkout 带 `submodules: recursive` + `fetch-depth: 0`，并安装 Dart Sass
- 中文站点配置：中文字数统计、阅读时间、归档与搜索入口
- `baseURL = https://czargz.github.io/hugo-blog/`

## 待提供的素材（仓库与地址已就绪，无需再给）

| 素材 | 状态 | 要求 |
|---|---|---|
| GitHub 用户名 + 仓库名 | ✅ 已定 `czargz / hugo-blog` | — |
| 站点标题与描述 | ⬜ | 当前为占位「我的技术博客」，给我正式标题与一句话描述 |
| 作者名 | ⬜ | 页脚与文章署名 |
| 文章正文 | ⬜ | 任意格式：Markdown / Word / PDF / 网页链接 / 纯文本，我来转换与排版 |
| 图片素材 | ⬜ | 文件本体或本机路径，我统一归档到 `static/images/` |
| 自定义域名 | ⬜ 可选 | 给域名即可，我写 `static/CNAME` 并改 `baseURL` |
| 头像 / Logo、社交链接、评论（Giscus）、统计代码 | ⬜ 可选 | 什么时候给都行，后加不改结构 |

## 日常使用流程

1. 本地预览：`./.tools/hugo/hugo.exe server -D`（工作区根目录执行）
2. 写文章：VS Code → Front Matter 面板 → 创建内容 → 目录选 `content/posts`
3. 发布：把 `draft: true` 改成 `draft: false`
4. 提交推送：VS Code 源代码管理 → 提交 → 推送
5. 等待 Actions 构建完成（约 1～2 分钟），线上自动更新

## 已踩过的坑（全部在本仓库修复）

1. **`actions/starter-workflows` 的 `hugo.yml` 已过时**：仍停留在 Hugo 0.128 + `snap install dart-sass`。本仓库用的是 Hugo 官方文档当前版。
2. **必须用 Hugo extended**：普通版编译 PaperMod 会因缺少 Sass 报错。
3. **Dart Sass 需单独安装**：新版 Hugo 不再内置；且解压前必须先 `mkdir -p "${HOME}/.local"`，否则 `tar` 直接失败。
4. **Pages 必须先开通**：`configure-pages` 的 `enablement: true` 只对已存在的站点生效，全新仓库仍需先调 API 用 `build_type: workflow` 开通一次，否则报 `Create Pages site failed`。
5. **`workflow_dispatch` 与 rerun 对旧运行无效**：早期那次 0-job 失败运行无法重跑，只能推新 commit 触发新运行。
6. **`fetch-depth: 0` + `submodules: recursive`**：否则主题拉不下来、`.Lastmod` 与 `.GitInfo` 取不到。
7. **`baseURL` 要带子路径**：项目页是 `https://用户名.github.io/仓库名/`，漏掉子路径会导致 CSS/图片全部 404。
8. **Hugo v0.158 起 `languageCode` 弃用**，须改用 `locale`；网上大量老教程仍在用旧写法。
9. **`.frontmatter/database/` 要提交**：内含图片元数据（`mediaDb.json`），删了会丢失图片标题与 alt 信息。
