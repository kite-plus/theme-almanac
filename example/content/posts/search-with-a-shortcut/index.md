---
id: 01KQ0YZ8009S6QWTX7RQ1DA2PW
title: 按 ⌘K 搜索全站
slug: search-with-a-shortcut
status: published
created_at: 2026-04-25T08:00:00+08:00
published_at: 2026-04-25T08:00:00+08:00
description: 装上 Kite 的搜索插件，页头就多出一个搜索框，按 ⌘K 或 / 在浏览器里搜全站文章。
tags: [前端, 搜索, 插件]
categories: [技术]
cardSize: standard
---
## 搜索交给插件

Almanac 自己不带搜索，而是和 Kite 的官方搜索插件配合：站点装了插件，页头就出现一个搜索框；没装时，搜索框不会显示。

## 装上插件

```bash
kite plugin add search-0.1.0.zip
kite plugin enable search
```

也可以在后台的插件页上传压缩包、打开开关。启用之后，`kite.yaml` 里会多出一段：

```yaml
plugins:
  enabled:
    - search
```

## 怎么用

- 点页头的搜索框，或者在任意页面按 `⌘K`、`Ctrl K`、`/`
- 上下键选择结果，回车打开，`Esc` 关闭

索引在站点构建时写好，搜索全部在读者的浏览器里完成，不需要服务器。
