---
title: 'Hello World：用 Multica 把这个博客「说」起来'
description: '记录第一次用 Multica 多 agent 协作搭建独立博客 solfx.dev 的全过程，并顺便介绍一下 Multica 这个开源平台。'
publishDate: 2026-05-06
tags: ['multica', 'agent', 'astro', 'cloudflare-pages']
---

这是 [solfx.dev](https://solfx.dev) 的第一篇文章。它本身就是一个验收用例 —— 走通「写 Markdown → push → Cloudflare Pages 自动构建 → 上线」整条链路，顺便聊聊把它建起来的工具：[Multica](https://github.com/multica-ai/multica)。

## 今天发生了什么

从早上到现在，这个博客是这样被「说」出来的：我在 Multica 里开了一个 issue，把方向写清楚 —— 「不想花服务器钱、要个能写中文的极简博客」。然后几个 agent 接过去拆任务、互相在评论里讨论、再各自动手：

- 调研主题，候选列了 Astro Paper / Cactus / Theme Pure / Fuwari 等，最后定 [Astro Theme Pure](https://github.com/cworld1/astro-theme-pure)；
- 建 GitHub 仓库 `solfx/solfx-blog`，跑通脚手架；
- 用 Cloudflare API 创 Pages 项目、绑域名 `solfx.dev`、自动签 HTTPS；
- 把站点信息从主题默认值替换成「SolfX 学习日志」；
- 最后写下你正在看的这篇文章。

我在中间做的事情很少 —— 主要是回答几个二选一的问题（公开还是私有？apex 还是子域？），其它的拆分、推进、互相对账都是 agent 之间在 issue 评论里完成的。

## Multica 是什么

[Multica](https://github.com/multica-ai/multica) 是一个开源的 **多 agent 协作平台**。它的定位不是「再做一个 Cursor」，而是一层 **任务编排 + 协作上下文**：

- **issue 是 agent 的工作单元**。和人协作一样，把目标、依赖、验收标准写在 issue 里，agent 拉走任务、写代码、贴结果。
- **多个 agent 可以同时在线**，分工协作（这次就是 No.1 CC 做规划、No.3 CCB 跑 Cloudflare）。它们通过 issue 评论互相 `@mention` 来协调。
- **Autopilot 把流程自动化**。可以让 agent 按计划/触发条件自动跑（比如每天扫一次仓库、有新 issue 自动建子任务）。
- **Runtime 跑在你自己机器上**。Mac 桌面 app 或者 CLI 都行，代码、API key 都不出本机。

简单一句：把日常那种「开 issue → 派活 → 等结果」的流程，原样搬给 AI agent 用。

## 这一套到底好在哪

我自己的体感：**它把"使用 LLM 写代码"从一个对话变成了一个工作流**。和 agent 单聊容易陷在长上下文里（你也累、它也容易跑偏），但只要把任务拆成 issue，每个 issue 自带验收标准、自带历史评论，agent 接手时上下文就是干净的，反复迭代代价很低。

而且它是开源的：[github.com/multica-ai/multica](https://github.com/multica-ai/multica) ，自己跑、自己审、不放心可以读源码 —— 对独立开发者比较友好。

下一篇大概会写写今天踩到的几个具体坑（CF API token 权限、自定义域名 DNS、Astro Pure 主题中文化），算是给后来人留点记号。

—— 写于 solfx.dev 上线第 0 天
