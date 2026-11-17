# 地址发布页

一个 5 KB 的单文件静态页面，用于"收藏不迷路"的地址发布页。
零外部资源、零构建步骤、零依赖，部署到 Cloudflare Pages。

## 特性

- **单文件** — 全部 HTML/CSS 内联，无 JS 文件、无字体、无图片请求
- **5.34 KB** — 小于一张缩略图，Brotli 压缩后约 2 KB
- **首屏 1 个请求** — 没有外部资源，无队头阻塞
- **响应式** — 手机 / 平板 / 桌面自适应
- **无障碍** — 语义化标签、focus 可见、aria-label
- **边缘缓存** — Cloudflare 全球 300+ 节点，TTFB 通常 < 100 ms

## 文件说明

| 文件 | 作用 |
| --- | --- |
| `index.html` | 页面本体，所有内容都在这里 |
| `_headers` | Cloudflare Pages 的响应头配置（安全头 + 缓存策略） |
| `_redirects` | 重定向规则（当前为空，留作扩展） |
| `.gitignore` | 排除系统与编辑器文件 |
| `CNAME.example` | 仅 GitHub Pages 需要，Cloudflare Pages 用不到 |

> `_headers` 和 `_redirects` 是 Cloudflare Pages 的**约定文件名**，必须放在**构建输出根目录**（本项目即仓库根目录），Cloudflare 会自动识别，不需要任何配置。

## 本地预览

直接用浏览器打开 `index.html`，或起本地服务：

```bash
python -m http.server 8080
# 或
npx serve .
```

访问 http://localhost:8080

## 修改内容

全部在 `index.html` 里：

| 改什么 | 找哪里 |
| --- | --- |
| 站点列表 | `<nav class="card">` 里的 `<a class="item">`，改 `href`、`.name`、`.url` |
| 图标文字 | `<div class="ico">主</div>` 里的单个汉字 |
| 顶部 Logo | `<div class="logo">時</div>` |
| 标题 / 副标题 | `<h1>` 和 `<p class="sub">` |
| 提示说明 | `<div class="note">` |
| 更新日期 | `<span id="upd">` |
| 配色 | `:root` 里的 CSS 变量 `--accent` `--bg` `--card` |

## 部署到 Cloudflare Pages

### 前置条件

- 一个 GitHub 账号
- 一个 Cloudflare 账号（免费即可，https://dash.cloudflare.com/sign-up）

### 步骤

1. **把仓库推到 GitHub**（见下方"Git 推送"章节）

2. **登录 Cloudflare Dashboard** → 左侧菜单 **Workers & Pages**

3. 点 **Create** → 选 **Pages** 标签 → **Connect to Git**

4. 授权 GitHub，选中 `release-page` 仓库 → **Begin setup**

5. 填写构建配置：

   | 字段 | 填什么 |
   | --- | --- |
   | Project name | `release-page`（决定默认域名 `release-page.pages.dev`） |
   | Production branch | `main` |
   | Framework preset | **None** |
   | Build command | **留空** |
   | Build output directory | **留空**（或填 `/`） |
   | Root directory | **留空** |

6. 点 **Save and Deploy**

   第一次部署约 30 秒。完成后给你一个 `https://release-page.pages.dev` 的地址。

7. **以后每次 `git push`，Cloudflare 自动重新部署**，无需手动操作。

### 分支预览（免费特性）

推送到非 `main` 分支（如 `dev`）时，Cloudflare 会自动生成一个独立的预览域名：
`https://dev.release-page.pages.dev`

可以在 Production 上线前先验证效果。

## 绑定自定义域名

### 情况 A：域名已经托管在 Cloudflare（推荐）

如果 `xakj.eu.org` 的 NS 已指向 Cloudflare：

1. Pages 项目 → **Custom domains** → **Set up a custom domain**
2. 输入 `release.xakj.eu.org` → **Continue**
3. Cloudflare 自动创建 CNAME 记录（因为域名就在同一个账号里），点 **Activate domain**
4. 等 1~2 分钟，证书自动签发

**不需要手动加 DNS 记录，Cloudflare 全自动处理。**

### 情况 B：域名托管在别处（DNSPod / 阿里云等）

1. Pages 项目 → **Custom domains** → 输入 `release.xakj.eu.org`
2. Cloudflare 提示你需要添加 CNAME，把它显示的目标值复制下来
3. 去你的 DNS 服务商添加：

   | 主机记录 | 类型 | 记录值 | TTL |
   | --- | --- | --- | --- |
   | `release` | CNAME | `release-page.pages.dev` | 600 |

4. 回 Cloudflare 点 **Check DNS** 验证

### ⚠️ 关于橙色云朵（代理）

| 场景 | 建议 |
| --- | --- |
| 域名托管在 Cloudflare + 绑定 Pages | **保持橙色云（已代理）**，这是 Cloudflare 官方推荐，能享受 CDN 和 WAF |
| 域名托管在别处，CNAME 到 `*.pages.dev` | 无法开代理，只能走 DNS |

> 注意：这和**用 Vercel 时的情况相反**。Vercel 必须设灰色云（否则证书冲突），
> 而 Cloudflare Pages 是自家产品，橙色云完全没问题，而且更快。

## Git 推送

```bash
cd release-page

# 首次
git remote add origin https://github.com/你的用户名/release-page.git
git push -u origin main

# 以后更新
git add .
git commit -m "更新站点链接"
git push
```

推送时密码栏填 **Personal Access Token**（不是账号密码）。

## 性能目标

| 指标 | 实测值 |
| --- | --- |
| 页面体积 | 5.34 KB |
| 外部请求数 | 0 |
| 首屏请求数 | 1 |
| 预期 TTFB（Cloudflare 边缘命中） | < 100 ms |

## 常见问题

**Q: 部署成功但页面 404？**
A: 检查 `index.html` 是否在**仓库根目录**。如果在子目录里，把 Build output directory 填成那个子目录名。

**Q: 改了文件但网站没变？**
A: 浏览器缓存。`Ctrl + F5` 强制刷新，或在 Cloudflare 后台 **Caching → Purge Everything**。

**Q: 自定义域名一直 Pending？**
A: DNS 还没生效。`nslookup release.xakj.eu.org` 确认解析是否正确。通常 5~30 分钟。

**Q: 想同时部署到 Vercel 做备份？**
A: 可以。加回一个 `vercel.json` 即可，两个平台能同时从同一个仓库部署，互不影响。

## License

MIT
