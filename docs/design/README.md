# 年鉴（Almanac）主题规划

> 状态：0.1.0，已发布，要求 Kite 0.1.2；§6 的三个 Kite 问题都已在 Kite 0.1.2 修复 · 最近更新：2026-09-29
> 文档约定沿用 [Kite 设计文档](https://github.com/kite-plus/kite/blob/main/docs/design/README.md#文档约定)：正文中文，专有名词保留英文；`[待定]` 表示明确推迟决策。
> 主题名 `almanac`，中文名年鉴；仓库 [`kite-plus/theme-almanac`](https://github.com/kite-plus/theme-almanac)。

年鉴是一套个人站点主题：杂志风刊头下是一列卡片，另有项目、书影、动态、友链和简历页，白天是奶油色的纸，夜里是暖色的墨。本文记下它的页面、设置、外观和实现上的取舍，以及 Kite 为它还要补的能力。

---

## 1. 定位

面向个人站点：除了文章，还想放上在做的项目、读过的书和看过的电影、随手记下的动态、友链和一份简历。

它和默认主题「草木集」、文档主题风标并列：草木集是安静的衬线博客，风标是文档站，年鉴的信息更密，用到的东西——列表里的封面、页面级的结构化数据——正好是另外两套没有碰到的契约缺口（§5）。

不做的事：文档站（风标负责）、多语言切换、在线编辑。

## 2. 页面

| 页面 | 内容 |
|---|---|
| 首页 | 期号、带状态绿点的头像、标题、职业和所在地、简介、社交链接和座右铭，上面一张横幅，页头浮在横幅上；下面是最新文章的卡片和「查看更多」。Kite 的首页随文章分页，第 2 页起画成普通的卡片列表加翻页 |
| 文章 | 分类、标题、日期 / 阅读时间 / 字数 / 更新日期、正文、标签、上一篇和下一篇、评论位；宽屏时旁边有目录，标题滚出屏幕后页头下方出现阅读条 |
| 归档 | `/posts/`，按年分组，跟随分页；想一页看全，调大 `build.pageSize` |
| 标签和分类 | 标签云的字号随文章数变化，由 CSS 从 `--count` / `--max` 算出；分类画成卡片 |
| 项目、书、电影 | 站点在 `kite.yaml` 里声明名为 `project`、`book`、`movie` 的内容类型（Kite 0.1.3 起），每一条是一个文件，后台有表单。列表 `/projects/`、`/books/`、`/movies/` 按状态分组，书和电影的列表之间有标签页互通；每一条有自己的页面：项目是图标、状态、简介、技术栈、链接、截图，正文旁边一栏仓库数据；书和电影是封面在左、作者或导演、年份、评分、状态和豆瓣链接在右，正文是读后感，底下是上一本、下一本。状态写在 `state` 里，因为 `status` 是 Kite 的发布状态 |
| 特殊页面 | 页面在 front matter 里写 `layout`：`about`、`projects`、`project`、`douban`、`moments`、`links`、`resume`、`feed`、`search`。要画的数据写在同一个页面的 front matter 里，每个布局的模板开头列着它读的字段。`projects`、`project`、`douban` 留给还没改用内容类型的站点 |
| 404 | 回首页，装了搜索插件时还能直接搜 |

## 3. 设置

| 分组 | 设置 |
|---|---|
| 个人资料 | 头像、职业、所在地、状态、首页标题、简介、座右铭、横幅、是否显示横幅、是否显示期号 |
| 外观 | 强调色（四种推荐色，夜里自动调亮）、白天或夜晚、网页字体、站点图标、分享图 |
| 导航 | 页头链接（名称、地址、手机菜单里的图标；站点写了主菜单时由菜单代替）、页头的 GitHub 按钮、订阅页 |
| 社交 | 社交链接：平台（28 个预设加「其他」）、地址、名称、图标、二维码、账号、说明、隐藏 |
| 首页 | 首页文章数、没有封面的文章画成什么样 |
| 文章 | 阅读时间、字数、更新日期、目录、阅读条 |
| 特殊页面 | GitHub 用户、GitHub 上的更多仓库（不列、Star 最多、最近推送）和最多列几个、关于页、简历页、项目列表的介绍、书架和影单的地址与介绍 |
| 页脚 | ICP 备案号、版权、是否注明 Kite 和 Almanac |

站点的作者和描述直接用 Kite 的 `site.author`、`site.description`，不在主题里重复。

## 4. 外观与实现

- **纸和墨**：白天是奶油色的纸、暖色的墨，强调色是赤陶橙；夜里是暖黑的底和浅色的字，表面越靠前越亮（页面、卡片、弹层、悬停）。字体是 Inter、Source Serif 4 和 JetBrains Mono，小标题用等宽的大写字母。
- **颜色 token 是完整的颜色**：`--brand: hsl(15 55% 53%)` 这样写，透明度一律用 `color-mix()`。强调色因此能在后台换，主题只在它不是默认值时写一段 `--accent-color`，夜里的颜色由它调亮得到。
- **夜间模式不依赖脚本**：`prefers-color-scheme` 加 `<html>` 上的 `data-theme`，自定义的 `dark` variant 两种都认，没有脚本时也跟随系统。页头的按钮在白天、夜晚、跟随系统之间切换，选择存在 `localStorage` 的 `kite-theme` 里，和 Kite 的其他主题共用；有 view transition 的浏览器里新配色从按钮处画圆铺开。
- **不靠脚本也能用**：后台的整站预览不运行主题脚本。手机菜单是一个复选框，社交二维码卡片是 `<details>`，书影的两个书架用单选按钮切换。
- **脚本只做增强**：`static/almanac/almanac.js` 负责页头在横幅上滚动后变实、阅读条和进度、目录跟随、代码块的语言栏和复制按钮、标题锚点、外链标记、宽表格横向滚动、看图器、复制按钮，以及项目的 GitHub 数据：Star、Fork、语言、许可证和最近提交，项目没写的简介、技术栈和官网用仓库的简介、话题和主页补上，「GitHub 上的更多仓库」按页面里的 `<template>` 画卡片。GitHub 返回的文字一律用 `textContent` 写进去。用户自己的仓库一次请求取完，别人的仓库单独请求，每页最多 8 个。
- **样式表预先编译**：`src/almanac.css` 用 Tailwind CSS v4 写，编译成 `static/almanac/almanac.css` 提交进仓库，站点不需要 Node。由值拼出来的 class 写进 `@source inline()`。
- **搜索和评论交给官方插件**：站点装了 [plugin-search](https://github.com/kite-plus/plugin-search) 时，页头出现搜索框，点开是插件的全文搜索，插件自己的浮动按钮不再出现；[plugin-comments](https://github.com/kite-plus/plugin-comments) 把评论放进主题留的 `data-kite-comments`，Waline 的配色绑到主题的颜色上。
- **不请求第三方**：字体默认用读者设备上的，打开 `web_fonts` 才加载 Google Fonts，而且不阻塞渲染。项目的 GitHub 数据由读者的浏览器去取（构建不能联网），只在设置了 `github_user` 时才取，缓存一小时。
- **主题自己说的字**都在 `_partials/t.html`：站点语言以 `zh` 开头用中文，其余用英文。后台里主题的中文在 `i18n/zh-CN.yaml`。
- **卡片的封面**：封面铺满卡片的一边，没有内边距和第二层边框。按图片是什么来画：照片铺满；Logo 放在一块纸色的底板上，图片用 `mix-blend-mode: multiply` 把自带的白底融进底板，不再是「色块里套白卡片再套 Logo」；截图像一张纸从按标签取色的底上升起来，露出顶部。`coverTint` 给底板或底色上色。
- **没有封面的卡片**：没写 `cover` 时用正文里的第一张图（`.Images`）。写了 `cover: false`，或者正文里也没有图，就画成纯文字的 compact 卡；设置「用第一个标签画一块封面」时，画成按标签取色的渐变色块。分享图也按同样的顺序取，最后退回站点分享图和头像。

## 5. 对 Kite 的依赖

对照 Kite 0.1.1（7396dac）整理，状态已更新到 Kite 0.1.2（cd8b496）：K1、K3、K4、K5、K8、K9 在 0.1.2 里修复了。主题里绕开 K8、K9 的写法还没有改，在 0.1.2 上照样能用。

| # | Kite 要做的 | 现状 | 对主题的影响 |
|---|---|---|---|
| K1 | **列表里的页面带上 front matter**（见 §6） | Kite 0.1.2 已修复（[kite#1](https://github.com/kite-plus/kite/issues/1)）；0.1.1 的列表和上一篇、下一篇只有标题、slug、摘要、日期和分类，`Params` 是空的，字数也没有 | 首页和标签页的卡片靠它画封面和阅读时间，`cardSize` 才起作用；同一版还加了正文图片 `.Images`，没写封面的文章用它的第一张。主题因此要求 Kite 0.1.2 |
| K2 | **站点声明的内容类型** | Kite 0.1.3 已有：`kite.yaml` 的 `content.types` | 主题画名为 `project`、`book`、`movie` 的内容类型，每条一个文件、一个表单、一个页面，同样的数据不用再写两遍。内容类型由站点声明，主题没法替站点声明，所以 README 给出一段照抄的配置。动态还写在页面的 front matter 里 |
| K3 | **发布 bundle 子目录里的文件**（见 §6） | Kite 0.1.2 已修复（[kite#2](https://github.com/kite-plus/kite/issues/2)）；0.1.1 只发布 bundle 顶层的文件 | 放在 `images/` 这类子目录里的图片和视频照常发布 |
| K4 | **中文标题的锚点**（见 §6） | Kite 0.1.2 已修复（[kite#3](https://github.com/kite-plus/kite/issues/3)）；0.1.1 的自动 id 只保留 ASCII，中文标题得到 `heading`、`heading-1`…… | 锚点就是标题的文字；目录脚本先 `decodeURIComponent` 再找标题，不用改 |
| K5 | **`aliases` 发布跳转页** | Kite 0.1.2 已修复（[kite#4](https://github.com/kite-plus/kite/issues/4)）：每个别名发布一个跳转页 | 改过地址的文章，旧地址跳到新地址 |
| K6 | **图片处理** | Kite 0.1.3 已有 `img.*`，只处理 bundle 里的图 | 主题还没用：封面可以是外部地址，做图出错会让整次构建失败。卡片封面以后可以先缩小再发布 |
| K7 | **模板里的 `T`** | 无 | 用 `t.html` 代替，同风标 T5 |
| K8 | **整数运算、类型转换** | Kite 0.1.2 已修复（[kite#12](https://github.com/kite-plus/kite/issues/12)）：参数都是整数时结果是整数，`coll.*` 接受任意列表，有 `math.Int`、`math.Float`；0.1.1 的 `math.*` 只收浮点数 | 主题仍从 `0.0` 开始计数、按字符串比较首页条数，标签云的字号交给 CSS `calc()`；这些写法可以换掉。同风标 §11 第 6、7 项 |
| K9 | **front matter 列表里的日期** | Kite 0.1.2 已修复（[kite#6](https://github.com/kite-plus/kite/issues/6)）：到模板里是时间，另有 `time.AsTime`；0.1.1 里是字符串 | 动态仍按长度识别几种写法再解析，可以换成直接格式化时间 |
| K10 | **首页不分页，或由主题决定** | 首页随文章分页 | 第 2 页起画成普通列表。同风标 §11 第 8 项 |
| K11 | **Markdown 扩展** | 没有 shortcode（Kite 设计文档 §14 待定） | 图片网格、Logo 网格和播放器要在文章里写 HTML，站点要开 `markdown.unsafeHTML` |

## 6. 待确认的 Kite 问题

K1、K3、K4 影响最大，三个都已在 Kite 0.1.2 修复（2026-09-29 发布）。下面保留发现时的记录，代码位置对应 Kite 0.1.1（7396dac）。

### K1 列表里的页面没有 front matter `[已修复]`

- **现象**：首页、文章列表、标签页里的卡片，以及文章底部的上一篇、下一篇，拿到的页面 `.Params` 是空的，`.WordCount` 是 0。卡片上没有封面，`cardSize` 不起作用；要是直接显示阅读时间，每篇都会是 1 分钟（主题在字数为 0 时不显示）。
- **复现**：在示例站里给一篇文章写上 `cover` 和 `cardSize: feature`，首页仍然把它画成没有封面的卡片；同一个值在这篇文章自己的页面上用 `.Page.Params.cover` 取得到。
- **位置**：`internal/build/build.go` 第 591 行的 `listedPage` 只读 `title`、`slug`、`excerpt`、`published_at`、`taxonomies`（第 595 行），第 742 行的 `summaryToContent` 不带 `Meta`；`content.Summary`（`internal/content/content.go` 第 93 行）本身就没有这些字段，只多一个 `Pinned`。
- **影响**：带封面的文章在卡片上看不到封面，只能画成纯文字卡片，或者主题画的标签色块。
- **可能的改法**：列表投影加上 `params` 和字数，存在索引里；构建记录依赖时把读到的这两个字段记上，列表仍然只读投影，不用加载正文。
- **修复**：[kite-plus/kite#1](https://github.com/kite-plus/kite/issues/1) 按上面的改法做了：列表查询带上每篇的 front matter，索引在记摘要时一并记下字数和正文图片，列表里的页面和上一篇、下一篇都有 `.Params`、`.WordCount` 和 `.Images`。Kite 0.1.2 起生效。

### K3 bundle 子目录里的文件不发布 `[已修复]`

- **现象**：`content/posts/x/images/a.jpg` 这样放在子目录里的文件不会发布，正文里的 `![](images/a.jpg)` 在构建出的站点和预览里都是坏图。
- **位置**：`internal/build/build.go` 第 347 行的 `MediaFiles` 只读 bundle 顶层，第 370 行遇到目录直接跳过。
- **影响**：图片多的文章不能把图片收进子目录，只能全部放在 bundle 顶层。
- **可能的改法**：Kite 递归发布子目录，子目录本身是另一个条目的 bundle 时不重复发布，扩展名 URL 风格下的冲突检查照旧。
- **修复**：[kite-plus/kite#2](https://github.com/kite-plus/kite/issues/2) 按这个改法做了，Kite 0.1.2 起生效。

### K4 中文标题的锚点 `[已修复]`

- **现象**：中文标题的 id 是 `heading`、`heading-1`、`heading-2`……目录能跳转，但地址里看不出是哪一节，调整标题顺序后同一节的锚点也会变。
- **复现**：示例站 `/posts/hello-world/` 的七个标题（「你好，世界」「段落」……「图片」），id 依次是 `heading` 到 `heading-6`。
- **位置**：`internal/render/markdown/markdown.go` 第 142 行用的是 goldmark 默认的 `parser.WithAutoHeadingID()`，它只保留 ASCII 字母和数字。
- **影响**：读者收藏或别处引用的锚点不稳定，改一下标题顺序就会失效。
- **可能的改法**：换一个保留 Unicode 字母和数字的 id 生成器，规则可以参照 GitHub 给标题生成锚点的方式（如 `#你好世界`）。这会改变所有现有站点的锚点，发版时要写明。
- **修复**：[kite-plus/kite#3](https://github.com/kite-plus/kite/issues/3) 按 GitHub 的规则从标题文字生成 id，`url.Anchorize` 用同一规则，Kite 0.1.2 起生效。示例站那七个标题的 id 现在依次是 `你好世界`、`段落`、`代码`、`引用`、`表格`、`列表`、`图片`。

## 7. 仓库

```
theme-almanac/
├── theme.yaml
├── layouts/            # 模板；主题自己说的话都在 _partials/t.html
├── static/almanac/     # almanac.css（编译产物）、almanac.js、图标，发布在站点的 /almanac/ 下
├── src/                # almanac.css 的 Tailwind 源文件和代码高亮配色
├── i18n/zh-CN.yaml     # 后台里主题的中文
├── screenshot.webp     # 1280×800，后台主题列表里用
├── example/            # 用这套主题的 Kite 站点，每种布局都有一个页面
├── scripts/package.sh  # 用 kite theme pack 打出后台能直接安装的 zip
├── package.json        # 只用来编译 CSS
├── LICENSE             # MIT
└── docs/design/        # 本文
```

- **开发**：在 `example/` 里 `kite run`，主题通过 `example/themes/almanac` 这个指向仓库根目录的符号链接接进去。改了模板里的 class 或 `src/` 之后跑 `npm run build`。
- **验收**：`kite theme verify .` 和 `(cd example && kite build --verify)` 都通过。
- **发版**：`scripts/package.sh` 调用 `kite theme pack`（要 Kite 0.1.5 及以上，`KITE` 可以指定别的 kite），打出 `dist/almanac-<版本>.zip`，打 tag 并附上这个 zip。

## 8. 待定问题

1. **字体**：要不要把三种字体的子集放进主题，做到既不请求第三方、又是设计时的字形 `[待定]`；中文字体太大，可能只放 Latin 子集。
