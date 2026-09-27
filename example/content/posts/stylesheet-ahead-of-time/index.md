---
id: 01KP770200CQDW5NXBBPSZ4Z7B
title: 样式表为什么是提前编译好的
slug: stylesheet-ahead-of-time
status: published
created_at: 2026-04-15T08:00:00+08:00
published_at: 2026-04-15T08:00:00+08:00
description: Almanac 的样式用 Tailwind CSS 写，编译好的样式表直接放在主题里，装主题的站点不需要 Node。
tags: [Tailwind, 构建, 前端]
categories: [技术]
---
## 主题里没有构建步骤

Kite 的主题就是一个目录：模板、静态文件、一个 `theme.yaml`。站点构建时不会替主题跑任何工具链，所以 Almanac 把 Tailwind 编译好的 `static/almanac/almanac.css` 直接放进仓库，装上就能用。

## 改了样式之后

改了模板里的 class，或者改了 `src/` 下的源文件，在主题目录里重新编译一次：

```bash
npm install
npm run build
```

Tailwind 只输出它在 `layouts/` 和 `almanac.js` 里找到的 class。

## 由值拼出来的 class

模板里用变量拼出来的 class，比如按列数拼的 `grid-cols-3`，扫描时是看不到的，要写进源文件的 `@source inline()`：

```css
@source inline("grid-cols-{1,2,3,4,5,6} gap-{1,2,3,4,5,6}");
```

## 读者那边

读者拿到的是一个压缩过的样式表，外加一个只做增强的小脚本：没有脚本时，页面照样能读、菜单照样能打开。
