# 小艾 · 地址发布页

> 收藏本页，换域名也不会迷路。

7.89 KB 单文件静态页面，零外部资源、零构建步骤、零依赖，部署在 Cloudflare Pages。

## 站点清单

| 站点 | 地址 |
| --- | --- |
| 小艾博客（主站） | https://xaxxkj.cn/ |
| 艾上签（内置证书） | https://xakj.ioszy.cn/ |
| 小艾商城 | https://www.kayougou.com/shop/xakj |
| 永久联系方式 | https://url.0qq.top/s/FqbwC |

## 特性

- **单文件** — 全部 HTML/CSS 内联，唯一的 JS 是 3 行年份更新
- **7.89 KB** — Brotli 压缩后约 2.5 KB
- **首屏 1 个请求** — 无外部 CSS/JS/字体/图片，零队头阻塞
- **响应式** — 320px ~ 4K 全适配，40px 触控热区
- **无障碍** — 语义化标签、`aria-label`、focus 可见、`prefers-reduced-motion` 降级
- **不收录** — `robots.txt` + `noindex` 双重屏蔽搜索引擎

## 文件说明

| 文件 | 作用 |
| --- | --- |
| `index.html` | 页面本体，所有内容与样式都在这里 |
| `_headers` | Cloudflare Pages 响应头（安全头 + 缓存策略） |
| `_redirects` | 重定向规则（当前为空，留作扩展） |
| `robots.txt` | 禁止搜索引擎收录 |
| `.gitignore` | 排除系统与编辑器文件 |
| `CNAME.example` | 仅 GitHub Pages 需要，Cloudflare Pages 用不到 |

> `_headers` / `_redirects` 是 Cloudflare Pages 的**约定文件名**，必须放在构建输出根目录（本项目即仓库根目录），Cloudflare 自动识别，无需任何配置。

## 本地预览

直接双击 `index.html`，或：

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
| 图标文字 | `<div class="ico">博</div>` 里的单个汉字 |
| 标签徽章 | `.name` 里的 `<span class="tag">主站</span>`，可选 `tag` / `new` / `hot` |
| 顶部 Logo | `<div class="logo">艾</div>` |
| 标题 / 副标题 | `<h1>` 和 `<p class="sub">` |
| 使用提示 | `<div class="note">` |
| 更新日期 | `<span id="upd">` |
| 配色 | `:root` 里的 CSS 变量 `--accent` `--bg` `--card` |

年份 `© 2026` 已用 JS 自动更新，不用每年改。

## 部署到 Cloudflare Pages

### 前置

- GitHub 账号
- Cloudflare 账号（免费版即可，https://dash.cloudflare.com/sign-up ）

### 步骤

1. **推代码到 GitHub**（见下方"Git 推送"）

2. 登录 Cloudflare Dashboard → 左侧 **Workers & Pages**

3. **Create** → 切到 **Pages** 标签 → **Connect to Git**

4. 授权 GitHub，选 `release-page` 仓库 → **Begin setup**

5. 构建配置：

   | 字段 | 填什么 |
   | --- | --- |
   | Project name | `release-page` |
   | Production branch | `main` |
   | Framework preset | **None** |
   | Build command | **留空** |
   | Build output directory | **留空** |
   | Root directory | **留空** |

   > 纯静态站，**构建命令和输出目录必须留空**，填了会失败。

6. **Save and Deploy**，约 30 秒完成，得到 `https://release-page.pages.dev`

7. 以后每次 `git push` 自动重新部署，无需手动操作。

### 分支预览

推到非 `main` 分支（如 `dev`）会自动生成 `https://dev.release-page.pages.dev`，线上验证后再合并。

## 绑定自定义域名

### 情况 A：域名已托管在 Cloudflare（推荐）

1. Pages 项目 → **Custom domains** → **Set up a custom domain**
2. 输入 `release.你的域名` → **Continue**
3. 同账号下 Cloudflare **自动创建 CNAME 记录**，直接点 **Activate domain**
4. 等 1~2 分钟，证书自动签发

**无需手动加 DNS 记录。**

### 情况 B：域名托管在别处（DNSPod / 阿里云等）

1. Pages 项目 → **Custom domains** → 输入域名
2. 复制 Cloudflare 给出的 CNAME 目标值
3. 到 DNS 服务商添加：

   | 主机记录 | 类型 | 记录值 | TTL |
   | --- | --- | --- | --- |
   | `release` | CNAME | `release-page.pages.dev` | 600 |

4. 回 Cloudflare 点 **Check DNS**

### ⚠️ 橙色云朵：和 Vercel 相反

| 场景 | 建议 |
| --- | --- |
| Cloudflare 托管域名 + 绑定 Pages | **保持橙色云（已代理）**，官方推荐，享受 CDN + WAF |
| 别处托管，CNAME 到 `*.pages.dev` | 无法开代理，只能走 DNS |

> 用 Vercel 时必须关橙色云（否则证书冲突）；用 Cloudflare Pages 则**要开**。这是两套方案最容易踩混的一点。

## Git 推送

```bash
cd release-page

# 首次
git remote add origin https://github.com/用户名/release-page.git
git push -u origin main

# 以后更新
git add .
git commit -m "更新站点链接"
git push
```

推送时密码栏填 **Personal Access Token**（不是账号密码）。

## 关于备案

- 本页部署在 **Cloudflare 海外节点**，**不需要 ICP 备案**
- 主站 `xaxxkj.cn` 已有备案（冀ICP备2020030162号），那是境内服务器必须的
- 如果将来把这个发布页也迁回境内服务器，则必须备案

## 性能目标

| 指标 | 实测值 |
| --- | --- |
| 页面体积 | 7.89 KB |
| 外部请求数 | 0 |
| 首屏请求数 | 1 |
| 预期 TTFB（Cloudflare 边缘命中） | < 100 ms |

## 常见问题

**Q: 部署成功但访问 404？**
A: `index.html` 必须在**仓库根目录**。若在子目录，Build output directory 填该子目录名。

**Q: 改了文件网站没变？**
A: 浏览器缓存。`Ctrl + F5` 强刷，或 Cloudflare 后台 **Caching → Purge Everything**。

**Q: 自定义域名一直 Pending？**
A: DNS 未生效，`nslookup release.你的域名` 确认解析。通常 5~30 分钟。

**Q: 想同时部署到 Vercel 做双活？**
A: 可以，同一仓库可被多个平台同时部署。但需加回 `vercel.json`，且 Vercel 那边的 Cloudflare 记录必须设灰色云。

## License

MIT
