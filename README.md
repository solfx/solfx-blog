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

## 给 Agent 的发文清单

被分派「写一篇文章」类任务的 Multica agent，按下面流程做就够。

**1. 拿到仓库**

仓库地址写死：`https://github.com/solfx/solfx-blog`，公开仓库，无需 token。

```bash
multica repo checkout https://github.com/solfx/solfx-blog   # 项目资源已挂时
git clone https://github.com/solfx/solfx-blog.git           # 兜底
```

> 如果 `multica repo checkout` 报 `repo is not configured for this workspace`，说明 Multica 项目 `solfx-blog` 还没挂这个 `github_repo` 资源；直接走 `git clone` 即可，并提醒维护者补挂资源。

**2. 写文章**

按上面「发一篇文章（两步）」那节，把文件放到 `src/content/blog/<slug>.md`，frontmatter 至少包含 `title` / `description` / `publishDate` / `tags`。`description` 控制在 160 字以内，`title` 控制在 60 字以内（content collection schema 强校验）。

**3. commit 用 GitHub no-reply 邮箱**

仓库公开，commit 邮箱会永久公开。统一用 no-reply 邮箱，**不要**改全局 git config：

```bash
git -c user.email='20767341+solfx@users.noreply.github.com' \
    -c user.name='solfx' \
    commit -m "post: <slug> — 一句话主题"
```

**4. push main，等 Cloudflare Pages 自动构建**

```bash
git push origin main
```

约 1–2 分钟后部署到 `solfx.dev`。

**5. 验证三件套**

```bash
SLUG=<slug>
curl -sIL "https://solfx.dev/blog/$SLUG" | grep -E '^HTTP|^location'   # 308 → 200
curl -s   "https://solfx.dev/rss.xml"        | grep -c "$SLUG"          # 1
curl -s   "https://solfx.dev/sitemap-0.xml"  | grep -c "$SLUG"          # 1
```

三项都过才算「发布成功」。回到触发任务的 Multica issue 评论里贴上线 URL + commit hash + 验证结果，结束。

---

## Cloudflare Web Analytics

**启用步骤（首次，在 CF Dashboard 操作一次）：**

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 选择账号 → **Web Analytics**
3. **Add a site** → 输入 `solfx.dev`
4. 选择 **Free, automatic setup**（CF Pages 项目无需改代码，自动注入）
5. 点击 **Done**

启用后访问几次 solfx.dev，再回 CF Web Analytics 控制台即可看到 PV/UV 数据。
