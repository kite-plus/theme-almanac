---
id: 01KP770200CQDW5NXBBPSZ4Z7B
title: Hugo Pipes：把 build chain 收进一个 hugo 命令
slug: hugo-pipes-pipeline
status: published
created_at: 2026-04-15T08:00:00+08:00
published_at: 2026-04-15T08:00:00+08:00
description: 为什么这个主题没有 webpack / vite / parcel —— Hugo Pipes + 内置的 css.TailwindCSS 一条命令搞定 CSS 和 JS。
tags: [Hugo, 构建, 前端]
categories: [技术]
---
## 静态站不需要打包器

Hugo 自带 `resources.Get` + `css.TailwindCSS` + `js.Build` + `minify` + `fingerprint`，这套 pipeline 加起来够用了：

```go-html-template
{{ with (templates.Defer (dict "key" "global")) }}
  {{ with resources.Get "css/main.css" }}
    {{ with . | css.TailwindCSS (dict "minify" true) | fingerprint }}
      <link rel="stylesheet" href="{{ .RelPermalink }}" integrity="{{ .Data.Integrity }}">
    {{ end }}
  {{ end }}
{{ end }}
```

这一段做了：

1. 等所有页面渲染完（`templates.Defer`），这时 `hugo_stats.json` 里已经记下了用到的每一个 class
2. 读 `assets/css/main.css`，交给 Tailwind CLI 按 `hugo_stats.json` 生成 utility 并 minify
3. 加 hash fingerprint + SRI integrity 头

不需要 dev server，不需要 manifest 解析。

## 唯一的 Node 依赖

只剩 `tailwindcss` 和 `@tailwindcss/cli` —— Hugo 负责调用，不需要 PostCSS，也不需要任何配置文件。

## Alpine.js 走 CDN

交互那一点点用 Alpine.js，直接 `<script defer src="https://unpkg.com/alpinejs">`，不进 build chain。这条线退化得可以。
