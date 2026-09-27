---
id: 01KQ0YZ8009S6QWTX7RQ1DA2PW
title: Pagefind 与 ⌘K：把搜索做成命令面板
slug: pagefind-quick-tour
status: published
created_at: 2026-04-25T08:00:00+08:00
published_at: 2026-04-25T08:00:00+08:00
description: 为什么不再上 Algolia，把搜索切回 Pagefind 的 ⌘K 命令面板 —— 静态、零依赖、零成本。
tags: [前端, 搜索, Pagefind]
categories: [技术]
cardSize: standard
---
## 静态站为什么不用 Algolia

之前用过几个赞助方案，后来觉得：

- 索引要爬，爬完了还得每次部署同步
- 免费额度对个人站来说虚多，但只要带宽稍涨就开始焦虑
- 客户端 JS 体积没省到哪里去，DOM 上挂的 widget 还得自己改样式

切到 Pagefind 之后，整条链路退化成一句命令：

```bash
hugo --gc --minify
npx pagefind --site public
```

构建出来的 `_pagefind/` 直接放静态资源里走 CDN，搜索全部在浏览器端跑。

## ⌘K 命令面板的实现

主题里只做了三件事：

1. `head.html` 里 lazy-load `/pagefind/pagefind-ui.js`
2. 全局监听 `Cmd/Ctrl+K`，打开一个 dialog
3. 把默认 UI 的样式改成与主题字体 / 配色一致

```js
window.addEventListener("keydown", (e) => {
  const isMac = navigator.platform.includes("Mac");
  if ((isMac ? e.metaKey : e.ctrlKey) && e.key === "k") {
    e.preventDefault();
    openSearch();
  }
});
```

## 索引大小

12 万字博客 → 索引约 280KB（gzip 后约 90KB）。第一次按 `⌘K` 时加载，之后驻留内存。
