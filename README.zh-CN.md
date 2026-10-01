# Almanac 年鉴

一套极简、内容向的 [Kite](https://github.com/kite-plus/kite) 个人站点主题：杂志风刊头下是一列卡片，另有项目、书影、动态、友链和简历页；白天是奶油色的纸，夜里是暖色的墨。

[English](README.md) · 简体中文

![Almanac 的首页，来自示例站点](screenshot.webp)

Almanac 是「年鉴 / 历书」——一本按年份记下日子和小事的书。这个主题也想做同样的事：给你读过、看过、做过、写过的东西留个痕迹。

## 画出来的样子

- **首页**：期号、带状态绿点的头像、标题、职业和所在地、简介、社交链接和座右铭，上面是一张横幅，页头浮在横幅上。下面是最新文章的卡片，最后是「查看更多」。
- **文章**：分类、阅读时间、字数和更新日期；宽屏时旁边有目录；标题滚出屏幕后，页头下方出现一条显示阅读进度的横条。
- **归档**：`/posts/` 按年分组；还有标签页和分类页。
- **特殊页面**：在页面的 front matter 里选一个布局——关于、项目、书影、动态、友链、可以直接打印成 A4 的简历、订阅页和搜索页。
- **代码块**有标明语言的标题栏和复制按钮；小标题带锚点；正文图片点开看大图；宽表格可以横向滚动。
- **白天和夜晚**：页头的按钮在白天、夜晚、跟随系统之间切换，浏览器会记住读者的选择。

每个页面不靠脚本也能用，Kite 后台的预览就是这个样子；除非你打开网页字体或者自己写了嵌入，不会从第三方加载任何东西。

## 使用

在 Kite 后台的 **设置 → 主题** 里把发布的 zip 拖到上传区，或者把它解压到站点的 `themes` 目录，再在 `kite.yaml` 里把 `theme.name` 设为 `almanac`。需要 Kite 0.1.4 或更高版本。

后台里的设置分为个人资料、外观、导航、社交、首页、文章、特殊页面和页脚几组，[示例站点的 kite.yaml](example/kite.yaml) 里大部分都填了。

页头画的是站点的主菜单，它写在 Kite 的 `kite.yaml` 里，在后台的 **设置 → 菜单** 里编辑。链接的 `icon` 参数决定它在手机菜单里的图标，比如 `params: {icon: book-open}`，不写就按地址猜一个。站点还没写主菜单时，页头显示「导航」里设置的链接。

### 特殊页面

页面在 front matter 里写 `layout`，或者在编辑器的「模板」菜单里选。布局要画的数据也写在同一个 front matter 里：

| 布局 | 画什么 | front matter |
|---|---|---|
| `about` | 个人资料、正文和社交链接 | 无 |
| `projects` | 按状态分组的项目；设置了 `github_user` 时显示 GitHub 数据 | `projects` |
| `project` | 一个项目自己的页面，项目页的条目用 `link` 指过来 | `status`、`summary`、`tech`、`repo`、`homepage`、`demo` |
| `douban` | 两个书架：书和影 | `books`、`movies` |
| `moments` | 按天分组的动态 | `moments` |
| `links` | 供别人复制的本站名片、申请方式和友链 | `groups`、`apply` |
| `resume` | 按 A4 排好的简历 | `profile`、`skills`、`experience`、`projects`、`education`、`summary`、`links` |
| `feed` | 订阅地址和复制按钮 | 无 |
| `search` | 打开搜索插件的按钮 | 无 |

例如一个项目页：

```yaml
---
title: 项目
layout: projects
projects:
  - title: fluxa
    status: active          # maintained、experimental、archived，或者任意一个词
    summary: 自托管的 AI 网关。
    tech: [Go, TypeScript]
    repo: https://github.com/you/fluxa
    homepage: https://fluxa.dev
---
```

每个布局的模板开头都列着它读取的全部字段，[示例站点](example/content/pages) 里每种布局都有一个页面。

### 写作

文章可以用 `cardSize` 指定它在首页的卡片样式：`feature`、`standard`、`right`、`compact` 或 `note`，用 `cover` 给一张封面（和文章放在同一个目录里的图片，或者一个地址），`coverMedium` 可以是 `photo`、`logo` 或 `shot`。没写封面的文章，卡片用正文里的第一张图；写了 `cover: false`（也就是 Kite 编辑器里的「不用封面」）就不用图。这些都需要 Kite 0.1.2 或更高的版本，它的列表才会把文章的 front matter 交给主题。

图片网格、Logo 网格，以及哔哩哔哩、YouTube、网易云音乐的播放器，都在文章里用 HTML 写，需要在 `kite.yaml` 里设 `markdown.unsafeHTML: true`。HTML 里面的 Markdown 前后各空一行：

```html
<div class="gallery" data-cols="3">

![](one.jpg)
![](two.jpg)
![](three.jpg)

</div>

<div class="embed"><iframe src="https://player.bilibili.com/player.html?bvid=BV1uv411q7Mv&autoplay=0" allowfullscreen></iframe></div>

<div class="embed music"><iframe src="https://music.163.com/outchain/player?type=2&id=1974443814&auto=0&height=66"></iframe></div>

<div class="pic-grid" data-cols="3">
  <div class="cell"><img src="kite.svg" alt="Kite"><span class="name">Kite</span></div>
  <div class="cell"><img src="go.svg" alt="Go"><span class="name">Go</span></div>
  <div class="cell"><img src="tailwind.svg" alt="Tailwind CSS"><span class="name">Tailwind CSS</span></div>
</div>
```

### 插件

搜索和评论交给 Kite 的官方插件。站点装了[搜索插件](https://github.com/kite-plus/plugin-search)后，页头会出现搜索框，点开是插件的全文搜索；装了[评论插件](https://github.com/kite-plus/plugin-comments)后，评论区显示在文章下方，配色跟着主题走，文章写 `comments: false` 就不显示。

## 开发

[`example/`](example) 是一个用这套主题的 Kite 站点，主题通过 `themes/almanac` 这个指向仓库根目录的符号链接接进去：

```sh
cd example
kite run
```

样式表是 Tailwind CSS，预先编译成 `static/almanac/almanac.css` 并提交进仓库，站点不需要 Node。改了模板里的 class 或 `src/` 下的文件之后：

```sh
npm install
npm run build
```

下面两条都通过，改动才算完成：

```sh
kite theme verify .
(cd example && kite build --verify)
```

## 发版

`scripts/package.sh` 打出 `dist/almanac-<版本>.zip`，里面只有一个名为 `almanac` 的目录，放着 `theme.yaml` 和主题的文件，后台可以直接安装。打 tag 并附上这个 zip。

## 设计

主题的页面、设置、外观，以及 Kite 还要补哪些能力，都写在 [docs/design/README.md](docs/design/README.md)。

## 许可

[MIT](LICENSE)。
