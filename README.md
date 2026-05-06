# solfx-blog

[SolfX 学习日志](https://solfx.dev) — 个人博客源代码。

基于 [Astro Theme Pure](https://github.com/cworld1/astro-theme-pure)（Apache-2.0）。

## 发一篇文章（两步）

**第一步：在 `src/content/blog/` 新建 `.md` 文件**

文件名即 URL slug，建议用英文小写加连字符，例如 `my-first-post.md`。

```markdown
---
title: '文章标题（最多 60 字）'
description: '一句话简介，显示在列表和 SEO meta（最多 160 字）'
publishDate: 2026-01-01
tags: ['标签1', '标签2']
---

正文从这里开始，支持标准 Markdown。
```

可选字段：

| 字段 | 说明 |
|------|------|
| `updatedDate` | 更新日期，格式同 `publishDate` |
| `draft: true` | 草稿，构建时不生成页面 |
| `heroImage` | 封面图，`{ src: './cover.png', alt: '描述' }` |
| `comment: false` | 关闭该篇文章的评论（默认开启） |

**第二步：push 到 main 分支**

```bash
git add src/content/blog/my-first-post.md
git commit -m "post: my-first-post"
git push
```

Cloudflare Pages 收到 push 后自动构建，约 1 分钟后 [solfx.dev](https://solfx.dev) 更新。

---

## 改站点信息

编辑 `src/site.config.ts`，常用字段：

| 字段 | 说明 |
|------|------|
| `theme.title` | 站点名 |
| `theme.author` | 作者署名 |
| `theme.description` | 站点描述（SEO） |
| `footer.social` | 页脚社交链接 |
| `header.menu` | 顶部导航 |

修改后同样 push main 即可自动部署。

---

## 本地预览

```bash
pnpm install
pnpm dev
# 打开 http://localhost:4321
```

## 构建

```bash
pnpm build   # 输出到 dist/
```

---

## Cloudflare Web Analytics

**启用步骤（首次，在 CF Dashboard 操作一次）：**

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 选择账号 → **Web Analytics**
3. **Add a site** → 输入 `solfx.dev`
4. 选择 **Free, automatic setup**（CF Pages 项目无需改代码，自动注入）
5. 点击 **Done**

启用后访问几次 solfx.dev，再回 CF Web Analytics 控制台即可看到 PV/UV 数据。
