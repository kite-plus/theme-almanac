# 年鉴（Almanac）主题规划

> 状态：0.1.0，未发布；§9 的三个 Kite 问题待确认 · 最近更新：2026-09-27
> 文档约定沿用 [Kite 设计文档](https://github.com/kite-plus/kite/blob/main/docs/design/README.md#文档约定)：正文中文，专有名词保留英文；`[待定]` 表示明确推迟决策。
> 主题名 `almanac`，中文名年鉴；仓库 `theme-almanac`，由 Hugo 主题 [amigoer/almanac](https://github.com/amigoer/almanac) 移植而来。

年鉴是一套个人站点主题：杂志风刊头下是一列卡片，另有项目、书影、动态、友链和简历页，白天是奶油色的纸，夜里是暖色的墨。它原本是 Hugo 主题，驱动着 [amigoer.com](https://www.amigoer.com)；这次按 Kite 的主题契约整套重写，目标是让 amigoer.com 整站迁到 Kite 上，外观不变。本文记下移植时的取舍、Kite 为它还要补的能力，以及迁移 amigoer.com 的步骤。

---

## 1. 为什么是它

- **默认主题之外的第三套主题**。默认主题「草木集」是安静的衬线博客，风标是文档站；年鉴是信息更密的个人站：卡片、书架、动态、简历。它用到的东西——列表页里的封面、自定义的内容分区、页面级的结构化数据——正好是另外两套没有逼出来的契约缺口（§5）。
- **真实站点的迁移**。amigoer.com 有 14 篇文章、8 个项目、书影、动态、友链、简历和 Waline 评论，迁移本身就是对 Kite「从 Hugo 搬过来」这件事的一次完整验收。

## 2. 和 Hugo 版的对应

| Hugo 版 | Kite 版 | 说明 |
|---|---|---|
| `layouts/index.html` 首页 | `home.html` | 刊头 + 最近文章。Kite 的首页随文章分页，第 2 页起画成普通的卡片列表加翻页 |
| `_default/single.html` | `single.html` | 分类、标题、日期 / 阅读时间 / 字数 / 更新日期、正文、标签、上一篇和下一篇、评论位 |
| `_default/archives.html` | `list.html`（`/posts/`） | 模板拿不到全站页面，归档改由文章列表页承担：按年分组，跟随分页。想一页看全，调大 `build.pageSize` |
| `taxonomy.html` / `terms.html` | `term.html` / `taxonomy.html` | 标签云按数量缩放字号，大小由 CSS 从 `--count` / `--max` 算出 |
| `about` / `links` / `douban` / `feed` / `search` / `resume` 布局 | `page/<同名>.html` | 页面在 front matter 里写 `layout: about` 等。数据从 `data/*.yaml` 挪进这个页面自己的 front matter |
| `projects/`、`reading/`、`watching/`、`moments/` 分区 | `page/projects.html`、`page/douban.html`、`page/moments.html` | Kite 只有 post 和 page 两种内容；一个分区改成一个页面，条目写在它的 front matter 里（§5 K2） |
| Alpine.js（unpkg） | `static/almanac/almanac.js` | 原生脚本，只做增强：页头滚动、阅读条、目录高亮、代码块、标题锚点、外链、表格、看图、社交卡片、复制、GitHub 数据 |
| 抽屉菜单、社交二维码、书影 tab | 复选框、`<details>`、单选按钮 | 不靠脚本也能用。后台的整站预览不运行主题脚本，看到的就是没有脚本时的样子 |
| Fancybox（jsDelivr） | 自带的 `<dialog>` 看图器 | 正文图片和动态的九宫格点开看大图，左右键切换 |
| Pagefind ⌘K | 官方搜索插件 | 站点装了 [plugin-search](https://github.com/kite-plus/plugin-search) 时，页头出现搜索框，点开是插件的全文搜索；没装时不显示 |
| Waline | 官方评论插件 | [plugin-comments](https://github.com/kite-plus/plugin-comments) 把评论放进主题留的 `data-kite-comments`；Waline 的配色绑到主题的颜色上 |
| Google Fonts | 设置 `web_fonts`，默认关 | 默认用读者设备上的字体，不请求第三方；打开后不阻塞渲染地加载 |
| 构建时从 GitHub 拉项目数据 | 读者的浏览器去拉 | 构建不能联网（构建纯度）。设置了 `github_user` 时，项目卡片的星标、复刻、语言和推送时间由浏览器请求，缓存一小时 |
| Tailwind v4 + `hugo_stats.json` | Tailwind v4 预编译 | `src/almanac.css` 编译成 `static/almanac/almanac.css` 提交进仓库，站点不需要 Node |
| `i18n/*.toml` + `i18n` 函数 | `_partials/t.html` | Kite 的模板还没有 `T`（风标 T5），主题自己的字全在一个文件里，中文站用中文，其余用英文 |
| WebP 多档与 `srcset` | 无 | Kite 没有图片处理（§5 K7），图片按上传的原样输出 |
| shortcode | 原始 HTML | 见 README 的「写作」一节；需要站点开 `markdown.unsafeHTML` |

## 3. 设置

| 分组 | 设置 |
|---|---|
| 个人资料 | 头像、职业、所在地、状态、首页标题、简介、座右铭、横幅、是否显示横幅、是否显示期号 |
| 外观 | 强调色（四种推荐色，夜里自动调亮）、白天或夜晚、网页字体、站点图标、分享图 |
| 导航 | 页头链接（名称、地址、手机菜单里的图标）、页头的 GitHub 按钮、订阅页 |
| 社交 | 社交链接：平台（28 个预设加「其他」）、地址、名称、图标、二维码、账号、说明、隐藏 |
| 首页 | 首页文章数、没有封面的文章画成什么样 |
| 文章 | 阅读时间、字数、更新日期、目录、阅读条 |
| 特殊页面 | GitHub 用户、关于页、简历页 |
| 页脚 | ICP 备案号、版权、是否注明 Kite 和 Almanac |

Hugo 版的 `params.author` 由站点作者 `site.author` 代替，`params.description` 由站点描述代替，`params.icp` 等都有同名或近似的设置。

## 4. 外观

沿用 Hugo 版的全部视觉：奶油纸 + 暖墨 + 赤陶橙（shadcn 的 Claude 配色），Inter / Source Serif 4 / JetBrains Mono，mono 大写小标题，卡片、chip、按钮、tab 的形状和交互都不变。改动只有三处：

- **颜色 token 改成完整的颜色**。Hugo 版写的是 `--brand: 15 55% 53%` 再用 `hsl(var(--brand) / .4)`；为了让强调色能在后台换，token 改成 `--brand: hsl(15 55% 53%)`，透明度一律用 `color-mix()`（Tailwind v4 本来就这么生成）。
- **夜间模式不再依赖脚本**。Hugo 版靠脚本给 `<html>` 加 `.dark`；现在是 `prefers-color-scheme` 加 `data-theme`，自定义的 `dark` variant 两种都认，没有脚本时也跟随系统。切换按钮的顺序仍是白天 → 夜晚 → 跟随系统，选择存在 `localStorage` 的 `kite-theme` 里，和 Kite 的其他主题共用。
- **没有封面的卡片**。Kite 目前的列表拿不到 front matter（K1），首页卡片默认画成纯文字的 compact 卡；设置「用第一个标签画一块封面」时画成 Hugo 版的渐变标签块。

## 5. 对 Kite 的依赖

对照 Kite 0.1.1（7396dac）整理。前三条决定 amigoer.com 能不能「原样」迁过来。

| # | Kite 要做的 | 现状 | 对主题和迁移的影响 |
|---|---|---|---|
| K1 | **列表里的页面带上 front matter**（待确认，见 §9） | 列表和上一篇、下一篇只有标题、slug、摘要、日期和分类，`Params` 是空的，字数也没有 | **首页卡片没有封面、没有阅读时间，`cardSize` 不起作用**。主题已经按有 `Params` 写好（封面、feature / standard / right / compact / note 五种卡片），Kite 一给就生效。建议列表投影加上 `params`，或至少 `cover`、`cardSize`、字数 |
| K2 | **站点声明的内容类型** | 只有 post 和 page | 项目、书影、动态只能写在一个页面的 front matter 里，没有各自的详情页和地址。和风标的 T1 是同一件事 |
| K3 | **发布 bundle 子目录里的文件**（待确认，见 §9） | 只发布 bundle 顶层的文件，`images/` 之类的子目录被跳过 | amigoer.com 有 5 篇文章的图片放在 `images/` 里（成都游记就有 29 张图和 2 段视频），迁移时要么摊平，要么等 Kite 发布子目录 |
| K4 | **中文标题的锚点**（待确认，见 §9） | 自动 id 只保留 ASCII，中文标题得到 `heading`、`heading-1`…… | 目录能用，但锚点难看，改标题顺序会变；Hugo 版的 `#段落` 这类旧链接会失效 |
| K5 | **`aliases` 发布跳转页** | 读取并保存，但构建不写跳转页 | amigoer.com 的 `/travel/chengdu-trip/`、`/posts/ai-accounts-journey/` 两个旧地址需要在托管平台上配跳转 |
| K6 | **把 Hugo 的 `summary` 当作描述** | 只认 `description` | 迁移时把 `summary` 改名为 `description` |
| K7 | **图片处理** | 无（设计文档里是 M5） | 没有 WebP 多档和 `srcset`；原图直出，大图要在迁移前压缩 |
| K8 | **模板里的 `T`** | 无 | 用 `t.html` 代替，同风标 T5 |
| K9 | **整数运算、类型转换** | `math.*` 只收浮点数，`coll.*` 只收 `[]any`，没有 `int` 转 `float` | 计数从 `0.0` 开始加；标签云的字号交给 CSS `calc()`；首页条数按字符串比较。同风标 §11 第 6、7 项 |
| K10 | **嵌套的日期** | front matter 列表里的日期是字符串 | 动态按长度识别几种写法再解析，其他写法不显示星期 |
| K11 | **首页不分页，或由主题决定** | 首页随文章分页 | 第 2 页起画成普通列表。同风标 §11 第 8 项 |
| K12 | **shortcode 或 Markdown 扩展** | 无（设计文档 §14 待定） | `gallery`、`music-163`、`video-*`、`pic-grid` 改写成 HTML；以后也可以由一个 `transform_markdown` 插件展开 |
| K13 | **Atom 和旧的订阅地址** | 只写 `/rss.xml` | Hugo 版的 `/index.xml`、`/atom.xml` 需要在托管平台上跳转到 `/rss.xml` |

## 6. 迁移 amigoer.com

按顺序做，前四步不需要等 Kite。

1. **站点**：在 `amigoer-blog` 旁边建一个 Kite 站点，`site` 写标题、描述、`baseURL`、`language: zh-CN`、`author: Amigoer`、`timezone: Asia/Shanghai`，`markdown.unsafeHTML: true`；把这个主题的 zip 装进去。
2. **设置**：`params` 对照 §3 填进主题设置：`avatar: /resume/amigoer.png`、`banner: /images/banner.webp`、职业、所在地、状态、首页标题和简介、座右铭；`menu.main` 变成 `nav`（「归档」指向 `/posts/`）；`socialIcons` 变成 `social`（`name` → `platform`，`qr`、`id` → `qr`、`account`）；`icp`；`githubUser` → `github_user`；`feed_page: /feed/`、`resume_page: /resume/`。
3. **内容**：
   - 文章原样搬进 `content/posts/`，`summary` 改为 `description`（K6）；Kite 读得懂 `date`、`draft`、`lastmod`，保存时会改成自己的写法。
   - `interview/` 下的两篇变成文章，加一个分类，比如「求职复盘」；`{{< private >}}` 包着的内容拿掉，或者整篇设为草稿。
   - shortcode 按 README 的写法改成 HTML（K12）。
   - `about.md`、`links.md`、`douban.md`、`feed.md`、`search.md`、`resume.md` 放进 `content/pages/`，`layout` 不变；`data/links.yaml` 的 `groups` 写进 `links.md`，`data/resume.yaml` 整个写进 `resume.md` 的 front matter。
   - `projects/`、`reading/`、`watching/`、`moments/` 各合并成一个页面（K2）；项目的正文放不进卡片，要么删减成 `summary`，要么另写成文章再用 `link` 指过去。
   - `archives.md`、`tags/_index.md` 不再需要。
4. **插件**：装 [plugin-comments](https://github.com/kite-plus/plugin-comments)，服务选 Waline，地址填 `https://comment.amigoer.com`。插件和 Hugo 版一样用 `location.pathname` 作为评论的 path，文章地址不变时旧评论都还在。再装 [plugin-search](https://github.com/kite-plus/plugin-search) 代替 Pagefind，需要统计的话装 [plugin-analytics](https://github.com/kite-plus/plugin-analytics)。
5. **等 Kite 补上 K1、K3、K4** 再切域名，否则首页没有封面、带子目录的图片丢失、中文锚点失效。K5、K13 可以先在 Vercel 的 `vercel.json` 里配跳转。
6. **切换**：`kite build --verify` 通过后，按 Kite 的部署文档发布，旧的 Hugo 仓库留作备份。

## 7. 仓库

```
theme-almanac/
├── theme.yaml
├── layouts/            # 模板；主题自己说的话都在 _partials/t.html
├── static/almanac/     # almanac.css（编译产物）、almanac.js、图标，发布在站点的 /almanac/ 下
├── src/                # almanac.css 的 Tailwind 源文件和代码高亮配色
├── i18n/zh-CN.yaml     # 后台里主题的中文
├── screenshot.webp     # 1280×800，后台主题列表里用
├── example/            # 用这套主题的 Kite 站点，内容来自 Hugo 版的 exampleSite
├── scripts/package.sh  # 打出后台能直接安装的 zip
├── package.json        # 只用来编译 CSS
├── LICENSE             # MIT，和 Hugo 版一致
└── docs/design/        # 本文
```

- **开发**：在 `example/` 里 `kite run`，主题通过 `example/themes/almanac` 这个指向仓库根目录的符号链接接进去。改了模板里的 class 或 `src/` 之后跑 `npm run build`，Tailwind 只输出它在 `layouts/` 和 `almanac.js` 里找到的 class；由值拼出来的 class 要写进 `src/almanac.css` 的 `@source inline()`。
- **验收**：`kite theme verify .` 和 `(cd example && kite build --verify)` 都通过。
- **发版**：`scripts/package.sh` 打出 `dist/almanac-<版本>.zip`，打 tag 并附上这个 zip。

## 8. 待定问题

1. **仓库放在哪**：`kite-plus/theme-almanac`（官方主题）还是 `amigoer/theme-almanac`（个人主题）`[待定]`。`theme.yaml` 的 `homepage` 和页脚的链接暂按前者写。
2. **项目详情页**：K2 之前，项目的长文是改写成文章，还是先放弃 `[待定]`。
3. **字体**：要不要把三种字体的子集放进主题，做到既不请求第三方、又和 Hugo 版一模一样 `[待定]`；中文字体太大，可能只放 Latin 子集。

## 9. 待确认的 Kite 问题

K1、K3、K4 决定 amigoer.com 能不能原样迁过来。三个问题都只记录在这里，**还没有在 Kite 里改**：先确认问题和改法，再决定是改 Kite，还是在迁移时绕开。代码位置对应 Kite 0.1.1（7396dac）。

### K1 列表里的页面没有 front matter `[待定]`

- **现象**：首页、文章列表、标签页里的卡片，以及文章底部的上一篇、下一篇，拿到的页面 `.Params` 是空的，`.WordCount` 是 0。卡片上没有封面，`cardSize` 不起作用；要是直接显示阅读时间，每篇都会是 1 分钟（主题在字数为 0 时不显示）。
- **复现**：在示例站里给一篇文章写上 `cover` 和 `cardSize: feature`，首页仍然把它画成没有封面的卡片；同一个值在这篇文章自己的页面上用 `.Page.Params.cover` 取得到。
- **位置**：`internal/build/build.go` 第 591 行的 `listedPage` 只读 `title`、`slug`、`excerpt`、`published_at`、`taxonomies`（第 595 行），第 742 行的 `summaryToContent` 不带 `Meta`；`content.Summary`（`internal/content/content.go` 第 93 行）本身就没有这些字段，只多一个 `Pinned`。
- **影响**：amigoer.com 首页带封面的卡片（logo 图块、照片）全部变成纯文字卡片，或者主题画的标签色块。
- **可能的改法**：列表投影加上 `params` 和字数，存在索引里；构建记录依赖时把读到的这两个字段记上，列表仍然只读投影，不用加载正文。

### K3 bundle 子目录里的文件不发布 `[待定]`

- **现象**：`content/posts/x/images/a.jpg` 这样放在子目录里的文件不会发布，正文里的 `![](images/a.jpg)` 在构建出的站点和预览里都是坏图。
- **位置**：`internal/build/build.go` 第 347 行的 `MediaFiles` 只读 bundle 顶层，第 370 行遇到目录直接跳过。
- **影响**：amigoer.com 有 5 篇文章的图片放在 `images/` 里：chengdu-trip（31 个文件，其中 2 段视频）、open-source-job-invite（8）、claude-code-hands-on（5）、super-individual（5）、hello-world（1）。
- **可能的改法**：Kite 递归发布子目录，子目录本身是另一个条目的 bundle 时不重复发布，扩展名 URL 风格下的冲突检查照旧；或者迁移时把图片挪到 bundle 顶层，正文里的路径一起改。

### K4 中文标题的锚点 `[待定]`

- **现象**：中文标题的 id 是 `heading`、`heading-1`、`heading-2`……目录能跳转，但地址里看不出是哪一节，调整标题顺序后同一节的锚点也会变。
- **复现**：示例站 `/posts/hello-world/` 的七个标题（「你好，世界」「段落」……「图片」），id 依次是 `heading` 到 `heading-6`。
- **位置**：`internal/render/markdown/markdown.go` 第 142 行用的是 goldmark 默认的 `parser.WithAutoHeadingID()`，它只保留 ASCII 字母和数字。
- **影响**：Hugo 版保留中文（如 `#你好世界`），读者收藏或别处引用的锚点迁移后会失效。
- **可能的改法**：换一个保留 Unicode 字母和数字的 id 生成器，规则尽量和 Hugo 的 github 风格一致。这会改变所有现有站点的锚点，发版时要写明。
