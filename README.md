# 地址发布页

一个 5 KB 的单文件静态页面，用于"收藏不迷路"的地址发布页。
零外部资源、零构建步骤、零依赖。

## 特性

- **单文件** — 全部 HTML/CSS 内联，无 JS 文件、无字体、无图片请求
- **5.34 KB** — 小于一张缩略图，Brotli 压缩后约 2 KB
- **首屏 1 个请求** — 没有外部资源，无队头阻塞
- **响应式** — 手机 / 平板 / 桌面自适应，支持深色模式
- **无障碍** — 语义化标签、focus 可见、aria-label

## 本地预览

直接用浏览器打开 `index.html` 即可，或起一个本地服务：

```bash
# Python 3
python -m http.server 8080

# 或 Node
npx serve .
```

然后访问 http://localhost:8080

## 修改内容

所有可改的东西都在 `index.html` 里：

| 改什么 | 找哪里 |
| --- | --- |
| 站点列表 | `<nav class="card">` 里的 `<a class="item">`，改 `href`、`.name`、`.url` |
| 图标文字 | `<div class="ico">主</div>` 里的单个汉字 |
| 顶部 Logo | `<div class="logo">時</div>` |
| 标题 / 副标题 | `<h1>` 和 `<p class="sub">` |
| 提示说明 | `<div class="note">` |
| 更新日期 | `<span id="upd">` |
| 配色 | `:root` 里的 CSS 变量 `--accent` `--bg` `--card` |

## 部署

### 方式一：Vercel（推荐，本项目已配置 vercel.json）

1. 把本目录推到 GitHub 仓库
2. 打开 https://vercel.com/new ，选择该仓库，点 Import
3. Framework Preset 选 **Other**，Build Command 留空，Output Directory 留空
4. Deploy
5. 在项目 Settings → Domains 里绑定你的域名

### 方式二：GitHub Pages

1. 推到 GitHub 仓库
2. Settings → Pages → Source 选 `Deploy from a branch`
3. Branch 选 `main`，目录选 `/ (root)`，Save
4. 等 1~2 分钟，访问 `https://用户名.github.io/仓库名/`
5. 绑自定义域名：在 Pages 页填 Custom domain，同时在 DNS 加 CNAME 指向 `用户名.github.io`

### 方式三：Cloudflare Pages

1. 推到 GitHub 仓库
2. Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git
3. 选仓库，Build output directory 填 `/`，Build command 留空
4. Save and Deploy

## 自定义域名 DNS 配置

| 场景 | 记录类型 | 主机记录 | 记录值 |
| --- | --- | --- | --- |
| 子域名 → Vercel | CNAME | `release` | `cname.vercel-dns.com` |
| 子域名 → GitHub Pages | CNAME | `release` | `用户名.github.io` |
| 子域名 → Cloudflare Pages | CNAME | `release` | `项目名.pages.dev` |

> 用 Cloudflare 管 DNS 时，以上记录全部设为**仅 DNS（灰色云朵）**。
> 开橙色云代理会导致和平台证书冲突，且多一跳反而更慢。

## 性能目标

| 指标 | 实测 |
| --- | --- |
| 页面体积 | 5.3 KB |
| 外部请求数 | 0 |
| 首屏请求数 | 1 |
| 预期 TTFB（Vercel 边缘缓存命中） | < 100 ms |

## License

MIT
