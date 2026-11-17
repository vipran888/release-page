# 简约地址发布页

## 简介

这是一个简约风格的地址发布页面，源码来源于 [Shiguang-coding/release-page](https://github.com/Shiguang-coding/release-page)。
页面设计简洁明了，易于使用和定制。

绿色渐变背景 + 上浮气泡动画 + 白卡片列表，单文件 HTML，无构建步骤。

**在线预览**：部署后填写你的地址

## 使用方法

1. 修改 `index.html` 里的站点描述及网址信息（见下方"修改位置"）。
2. 将网站部署到服务器或静态托管平台。
3. 访问网站，开始使用。

## 修改位置

全部在 `index.html` 里，只有两处需要改：

### ① 站点列表（第 518~550 行附近）

```html
<ul>
    <li>
        小艾博客（主站）:
        <a href="https://xaxxkj.cn" target="_blank"><i>https://xaxxkj.cn</i></a>
    </li>
    ...
</ul>
```

- `<li>` 前面的纯文本 = 左侧的中文名
- `href="..."` = 实际跳转地址
- `<i>...</i>` = 右侧显示的网址（绿色小字）

复制一个 `<li>` 块就能加站点，删掉就减少。

### ② 页脚版权（第 555 行）

```html
<p class="footer">小艾 © 2024 xaxxkj.cn All Rights Reserved</p>
```

### 可选：标题与 SEO 信息（第 6~13 行）

```html
<meta name="Author" content="Noah">
<title>小艾站点发布页，收藏我回家不迷路！</title>
<meta name="keywords" content="..." />
<meta name="description" content="..." />
```

## 文件说明

| 文件 | 作用 |
| --- | --- |
| `index.html` | 页面本体，样式与内容全在这一个文件里 |
| `README.md` | 本说明文件 |
| `.gitignore` | 排除系统与编辑器文件 |

> 页面引用了两处外部资源，均为原版自带：
> - **阿里巴巴 iconfont 字体**（`at.alicdn.com`），用于列表项图标
> - 字体文件已 base64 内嵌一份 woff2，断网也能显示图标

## 本地预览

直接双击 `index.html`，或：

```bash
python -m http.server 8080
```

访问 http://localhost:8080

## 部署

### Cloudflare Pages（推荐）

1. 把本目录推到 GitHub 仓库
2. Cloudflare Dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
3. 选仓库 → 构建配置全部**留空**：
   - Framework preset: `None`
   - Build command: 留空
   - Build output directory: 留空
   - Root directory: 留空
4. **Save and Deploy**

得到 `https://项目名.pages.dev`。以后 `git push` 自动重新部署。

### 绑定自定义域名

项目 → **Custom domains** → **Set up a custom domain** → 输入域名 → 点 Activate。

域名托管在 Cloudflare 时**自动创建 CNAME**；在别处托管时手动添加：

| 主机记录 | 类型 | 记录值 |
| --- | --- | --- |
| `release` | CNAME | `项目名.pages.dev` |

### Vercel / GitHub Pages

同样适用，纯静态无需构建。GitHub Pages 记得在 Settings → Pages 里开启，并把分支设为 `main`。

## 与原版的差异

本仓库 fork 自 `Shiguang-coding/release-page`，**样式与结构完全保留**，仅改动：

| 项 | 原版 | 本版 |
| --- | --- | --- |
| 站点内容 | 時光的 5 个站点 | 小艾的 4 个站点 |
| 标题/SEO | 時光相关 | 小艾相关 |
| 页脚版权 | 時光 © 2024 | 小艾 © 2024 |
| CNZZ 统计脚本 | 有（原作者账号 1278893351） | **已移除** |
| `<link rel="favicon">` | 有（但仓库无此文件，404） | **已移除** |
| 搜索引擎收录 | 允许 | **禁止**（`noindex,nofollow`） |

> CNZZ 脚本会向原作者的统计账号上报你站点的访问数据，属于隐私问题，故移除。
> 若你需要统计，自行替换为友盟或 Google Analytics。

## 注意事项

- 本项目仅供学习和研究使用，请勿用于非法用途。
- 使用前请确保已阅读并理解相关开源协议。
- 如有任何疑问或问题，请随时联系我们。

## 免责声明

本项目的源码来源于网络，仅供参考和学习使用。对于因使用本项目而导致的任何损失或后果，作者概不负责。请在使用前充分了解并评估风险。
