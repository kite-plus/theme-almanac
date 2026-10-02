# Almanac 年鉴

一套极简、内容向的 [Kite](https://github.com/kite-plus/kite) 个人站点主题：杂志风刊头下是一列卡片，另有项目、书影、动态、友链和简历页；白天是奶油色的纸，夜里是暖色的墨。

[English](README.md) · 简体中文

![Almanac 的首页，来自示例站点](screenshot.webp)

Almanac 是「年鉴 / 历书」——一本按年份记下日子和小事的书。这个主题也想做同样的事：给你读过、看过、做过、写过的东西留个痕迹。

## 画出来的样子

- **首页**：期号、带状态绿点的头像、标题、职业和所在地、简介、社交链接和座右铭，上面是一张横幅，页头浮在横幅上。下面是最新文章的卡片，最后是「查看更多」。
- **文章**：分类、阅读时间、字数和更新日期；宽屏时旁边有目录；标题滚出屏幕后，页头下方出现一条显示阅读进度的横条。
- **归档**：`/posts/` 按年分组；还有标签页和分类页。
- **项目、书和电影**：每一个都是单独的文件，在后台有自己的表单、在站点有自己的页面；列表按状态分组，项目的 Star、简介和话题从 GitHub 取。
- **特殊页面**：在页面的 front matter 里选一个布局——关于、动态、友链、可以直接打印成 A4 的简历、订阅页和搜索页。
- **代码块**有标明语言的标题栏和复制按钮；小标题带锚点；正文图片点开看大图；宽表格可以横向滚动。
- **白天和夜晚**：页头的按钮在白天、夜晚、跟随系统之间切换，浏览器会记住读者的选择。

每个页面不靠脚本也能用，Kite 后台的预览就是这个样子；除非你打开网页字体或者自己写了嵌入，不会从第三方加载任何东西。

## 使用

在 Kite 后台的 **设置 → 主题** 里把发布的 zip 拖到上传区，或者把它解压到站点的 `themes` 目录，再在 `kite.yaml` 里把 `theme.name` 设为 `almanac`。需要 Kite 0.1.4 或更高版本。

后台里的设置分为个人资料、外观、导航、社交、首页、文章、特殊页面和页脚几组，[示例站点的 kite.yaml](example/kite.yaml) 里大部分都填了。

页头画的是站点的主菜单，它写在 Kite 的 `kite.yaml` 里，在后台的 **设置 → 菜单** 里编辑。链接的 `icon` 参数决定它在手机菜单里的图标，比如 `params: {icon: book-open}`，不写就按地址猜一个。站点还没写主菜单时，页头显示「导航」里设置的链接。

### 项目、书和电影

在 `kite.yaml` 里把它们声明成站点自己的内容类型（需要 Kite 0.1.3 及以上），主题会画名为 `project`、`book`、`movie` 的三种。之后每个项目、每本书、每部电影都是 `content/projects/`、`content/books/`、`content/movies/` 下的一个文件，在后台有表单，在站点有自己的页面，并出现在 `/projects/`、`/books/`、`/movies/` 的列表里：

```yaml
content:
  types:
    - kind: project
      label: 项目
      dir: projects
      order: weight              # 按每个项目的 weight 从小到大排
      taxonomies: [tech]         # 技术栈在编辑器里像标签一样添加
      fields:
        - {key: cover, type: image, label: 截图, help: 项目页顶部的大图。}
        - {key: description, type: text, label: 一句话介绍, help: 留空时从 GitHub 取仓库的简介。}
        - {key: icon, type: image, label: Logo, help: 卡片上项目名旁边的小图标。}
        - key: state
          type: select
          label: 状态
          default: active
          options:
            - {value: active, label: 进行中}
            - {value: maintained, label: 维护中}
            - {value: experimental, label: 实验}
            - {value: archived, label: 已归档}
        - {key: repo, type: url, label: 仓库, placeholder: "https://github.com/you/project"}
        - {key: homepage, type: url, label: 官网}
        - {key: demo, type: url, label: Demo}
    - kind: book
      label: 书
      dir: books
      fields:
        - {key: cover, type: image, label: 封面}
        - {key: description, type: text, label: 一句话短评}
        - {key: author, type: string, label: 作者}
        - {key: rating, type: number, label: 评分, min: 0, max: 5, step: 1}
        - key: state
          type: select
          label: 状态
          default: finished
          options:
            - {value: reading, label: 在读}
            - {value: finished, label: 读过}
            - {value: wishlist, label: 想读}
            - {value: abandoned, label: 弃读}
        - {key: year, type: string, label: 出版年}
        - {key: link, type: url, label: 豆瓣链接, placeholder: "https://book.douban.com/subject/..."}
    - kind: movie
      label: 影
      dir: movies
      fields:
        - {key: cover, type: image, label: 海报}
        - {key: description, type: text, label: 一句话短评}
        - {key: director, type: string, label: 导演}
        - {key: rating, type: number, label: 评分, min: 0, max: 5, step: 1}
        - key: state
          type: select
          label: 状态
          default: watched
          options:
            - {value: watching, label: 在看}
            - {value: watched, label: 看过}
            - {value: wishlist, label: 想看}
            - {value: abandoned, label: 弃剧}
        - {key: year, type: string, label: 上映年}
        - {key: link, type: url, label: 豆瓣链接, placeholder: "https://movie.douban.com/subject/..."}
```

**添加一本书**：在后台打开「书」，点「新建书」，把封面拖到标题上方，标题下面写一句短评，在下面的小标签里填作者、评分和状态，正文写读后感。发布的日期就是记下它的日期。电影在「影」里、项目在「项目」里，用同样的方法添加。字段名是站点自己写的，[示例站点的 kite.yaml](example/kite.yaml) 用的就是上面这一份。

书和电影按状态分组，两个列表上方各有一个标签页通向对方，地址在「特殊页面」的「书架」和「影单」里设置。封面可以是一个地址，比如豆瓣的图片，主题会不带 referrer 去请求，豆瓣只认这样的请求；没有封面的书，主题按书名的色调画一张。项目同样按状态分组。Kite 用 `status` 表示条目是否发布，所以这里写 `state`。

在「特殊页面」里填了 GitHub 用户之后，读者的浏览器会向 GitHub 取每个项目的 Star、Fork、语言、许可证和最近提交；项目没写的简介、技术栈和官网，也用仓库的简介、话题和主页补上——所以添加一个项目，最少只要写名字和仓库地址。「GitHub 上的更多仓库」会在项目下面列出这个用户的其他公开仓库，按 Star 最多或最近推送排。GitHub 的数据在读者的浏览器里缓存一小时，没填用户时什么也不请求。

原来把项目、书和电影写在页面 front matter 里的站点，把每一条挪成单独的文件，再删掉占着列表地址的页面，比如 `/projects/` 上的项目页。`projects`、`project`、`douban` 这三个布局仍然可以画这样的页面。

### 特殊页面

页面在 front matter 里写 `layout`，或者在编辑器的「模板」菜单里选。布局要画的数据也写在同一个 front matter 里：

| 布局 | 画什么 | front matter |
|---|---|---|
| `about` | 个人资料、正文和社交链接 | 无 |
| `projects` | 按状态分组的项目；设置了 `github_user` 时显示 GitHub 数据 | `projects` |
| `project` | 一个项目自己的页面，项目页的条目用 `link` 指过来 | `state`、`summary`、`tech`、`icon`、`cover`、`repo`、`homepage`、`demo` |
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
    state: active           # maintained、experimental、archived，或者任意一个词
    summary: 自托管的 AI 网关。
    tech: [Go, TypeScript]
    repo: https://github.com/you/fluxa
    homepage: https://fluxa.dev
---
```

每个布局的模板开头都列着它读取的全部字段，[示例站点](example/content/pages) 里大多数布局都有一个页面。

### 写作

文章可以用 `cardSize` 指定它在首页的卡片样式：`feature`、`standard`、`right`、`compact` 或 `note`，用 `cover` 给一张封面（和文章放在同一个目录里的图片，或者一个地址）。没写封面的文章，卡片用正文里的第一张图；写了 `cover: false`（也就是 Kite 编辑器里的「不用封面」）就不用图。

封面铺满卡片的一边，按 `coverMedium` 说的图片类型来画：

- `photo`：照片，铺满整个框。
- `logo`：Logo 放在一块底板上，Logo 图片自带的白底会融进底板里。
- `shot`：截图或者很长的图，像一张纸从有色的底上升起来，露出顶部。

不写时，SVG 和 `coverFit: contain` 的封面按 Logo 画，其余按照片画。`coverTint` 给 Logo 的底板或截图的底色上色，底板用浅色比较好看。

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

`scripts/package.sh` 调用 `kite theme pack`（要 Kite 0.1.5 及以上，`KITE` 可以指定别的 kite），打出 `dist/almanac-<版本>.zip`，里面只有一个名为 `almanac` 的目录，放着 `theme.yaml` 和主题的文件，后台可以直接安装。打 tag 并附上这个 zip。

## 设计

主题的页面、设置、外观，以及 Kite 还要补哪些能力，都写在 [docs/design/README.md](docs/design/README.md)。

## 许可

[MIT](LICENSE)。
