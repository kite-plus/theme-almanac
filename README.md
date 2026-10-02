# Almanac

A minimal, content-rich personal hub theme for [Kite](https://github.com/kite-plus/kite):
a magazine masthead over a feed of cards, and shelves for projects, books and
films, moments, friend links and a resume, on cream paper by day and warm ink
by night.

English · [简体中文](README.zh-CN.md)

![Almanac's home page, in the example site](screenshot.webp)

An almanac is a yearly book, a record of dates and small markers of the year
past. This theme is the same: a place to leave traces of what you read, watch,
build and write.

## What it draws

- **A home page**: an issue line, your avatar with a status dot, a headline,
  where you are and what you do, an introduction, your social links and a
  motto, under a banner the header floats over. Then the newest posts as
  cards and a link to all of them.
- **Posts** with their category, reading time, word count and the day they
  were updated, a table of contents beside them on a wide screen, and a bar
  under the header that shows how far a reader has read.
- **An archive** at `/posts/`, grouped by year, and tag and category pages.
- **Projects, books and films**, each a file of its own with a form in the
  studio and a page of its own, listed on shelves grouped by where each one
  is, with a project's stars, summary and topics fetched from GitHub.
- **Special pages**, each a page that names a layout in its front matter:
  about, moments, friend links, a resume that prints on A4, a page to
  subscribe and a page to search.
- **Code blocks** with a bar that names the language and a button that copies
  the code, headings with anchors, pictures that open in a viewer, and tables
  that scroll.
- **Day and night**: the button in the header steps through day, night and
  the reader's system, and the browser remembers the choice.

Every page works without a script, as Kite's preview in the studio shows it,
and nothing is loaded from a third party unless you turn on web fonts or
write an embed yourself.

## Using it

Drop the zip of a release on the upload tile under **Settings → Theme** in
Kite's studio, or unzip it into a site's `themes` folder and set `theme.name`
to `almanac` in `kite.yaml`. Almanac asks for Kite 0.1.4 or later.

Its settings are grouped in the studio as Profile, Look, Navigation, Social,
Home page, Posts, Special pages and Footer.
[The example site's kite.yaml](example/kite.yaml) sets most of them.

The header draws the site's main menu, which Kite keeps in `kite.yaml` and
the studio edits under **Settings → Menus**. A link's `icon` param picks its
icon in the menu of a narrow screen, as `params: {icon: book-open}`, and
otherwise one is guessed from its address. Until the site writes a main menu,
the header shows the links set under Navigation.

### Projects, books and films

A site declares them in `kite.yaml` as kinds of content of its own, which
Kite 0.1.3 and later read, and Almanac draws the kinds named `project`,
`book` and `movie`. Each item is then a file under `content/projects/`,
`content/books/` or `content/movies/`, with a form in the studio, a page of
its own and a place in a listing at `/projects/`, `/books/` or `/movies/`:

```yaml
content:
  types:
    - kind: project
      label: Projects
      dir: projects
      order: weight              # by each one's weight, smallest first
      taxonomies: [tech]         # tech as chips in the editor
      fields:
        - {key: cover, type: image, label: Screenshot}
        - {key: description, type: text, label: Summary, help: Left empty, the repository's own is shown.}
        - {key: icon, type: image, label: Icon}
        - key: state
          type: select
          label: State
          default: active
          options:
            - {value: active, label: Active}
            - {value: maintained, label: Maintained}
            - {value: experimental, label: Experimental}
            - {value: archived, label: Archived}
        - {key: repo, type: url, label: Repository}
        - {key: homepage, type: url, label: Website}
        - {key: demo, type: url, label: Demo}
    - kind: book
      label: Books
      dir: books
      fields:
        - {key: cover, type: image, label: Cover}
        - {key: description, type: text, label: A line on it}
        - {key: author, type: string, label: Author}
        - {key: rating, type: number, label: Rating, min: 0, max: 5, step: 1}
        - key: state
          type: select
          label: State
          default: finished
          options:
            - {value: reading, label: Reading}
            - {value: finished, label: Finished}
            - {value: wishlist, label: Want to read}
            - {value: abandoned, label: Abandoned}
        - {key: year, type: string, label: Year}
        - {key: link, type: url, label: Link, placeholder: "https://book.douban.com/subject/..."}
    - kind: movie
      label: Films
      dir: movies
      fields:
        - {key: cover, type: image, label: Poster}
        - {key: description, type: text, label: A line on it}
        - {key: director, type: string, label: Director}
        - {key: rating, type: number, label: Rating, min: 0, max: 5, step: 1}
        - key: state
          type: select
          label: State
          default: watched
          options:
            - {value: watching, label: Watching}
            - {value: watched, label: Watched}
            - {value: wishlist, label: Want to watch}
            - {value: abandoned, label: Abandoned}
        - {key: year, type: string, label: Year}
        - {key: link, type: url, label: Link, placeholder: "https://movie.douban.com/subject/..."}
```

To add a book, open **Books** in the studio and make a new one: drop its
cover on the space above the title, write a line on it under the title, set
its author, rating and state in the chips under that, and write what you
made of it in the text. The day it is published is the day it is noted. A
film is added the same way under **Films**, and a project under
**Projects**. The labels are the site's own, in its own language; the
[example site's kite.yaml](example/kite.yaml) writes them in Chinese.

The books and the films are grouped by state, and each listing has a tab
that leads to the other, set as Books and Films under Special pages. A
picture of a cover may be an address, such as one of Douban's, which is
asked for without a referrer as Douban requires; a book with no cover is
given one drawn in a tone of its title. Projects are grouped by state too.
Kite keeps `status` for whether an item is published, so these write
`state`.

With a GitHub user set under Special pages, the reader's browser asks GitHub
for each project's stars, forks, language, license and last push, and fills
in the summary, the tech and the website a project leaves out from its
repository's description, topics and homepage, so a project can be as short
as a title and a repository. More from GitHub lists the user's other public
repositories under the projects, the most starred or the most recently
pushed. What GitHub says is kept in the reader's browser for an hour, and
nothing is asked of it while no user is set.

A site that kept its projects, books and films in a page's front matter
moves each entry to a file of its own, and takes away the page whose
address the listing now has, such as a projects page at `/projects/`. The
`projects`, `project` and `douban` layouts still draw such pages.

### Special pages

A page chooses a layout in its front matter, or from the Template menu in the
editor. The data a layout draws is written in the same front matter:

| Layout | Draws | Front matter |
|---|---|---|
| `about` | Your profile over the page's text, and your social links | none |
| `projects` | Projects grouped by state, with what GitHub says of them when `github_user` is set | `projects` |
| `project` | One project's own page, which a projects entry can lead to with `link` | `state`, `summary`, `tech`, `icon`, `cover`, `repo`, `homepage`, `demo` |
| `douban` | Books and films on two shelves | `books`, `movies` |
| `moments` | Short entries grouped by day | `moments` |
| `links` | Your site's card to copy, how to ask for a link, and the links | `groups`, `apply` |
| `resume` | A resume laid out to print on A4 | `profile`, `skills`, `experience`, `projects`, `education`, `summary`, `links` |
| `feed` | The feed's address with a button that copies it | none |
| `search` | A button that opens the Search plugin's box | none |

For example, a projects page:

```yaml
---
title: Projects
layout: projects
projects:
  - title: fluxa
    state: active           # maintained, experimental, archived, or any word
    summary: A self-hosted gateway for AI models.
    tech: [Go, TypeScript]
    repo: https://github.com/you/fluxa
    homepage: https://fluxa.dev
---
```

Each layout's template starts with the full list of what it reads, and
[the example site](example/content/pages) has a page for most of them.

### Writing

A post can ask for the card it is drawn with on the home page with
`cardSize`: `feature`, `standard`, `right`, `compact` or `note`, and give a
`cover`, a picture beside it in its folder or an address. A post that names
no cover is drawn with the first picture of its text, and one with
`cover: false`, which is what the No cover button in Kite's editor writes,
with none.

A cover runs to the edges of its card, drawn by what it is, which
`coverMedium` says:

- `photo` fills the frame.
- `logo` sits on a plate, and the white box a logo is often drawn on melts
  into it.
- `shot`, a screenshot or a tall picture, rises as a sheet from a tinted
  ground, its top in view.

Left out, an SVG or a cover with `coverFit: contain` is a logo and anything
else a photo. `coverTint` colors the plate of a logo or the ground of a
shot; a pale color suits a plate.

Pictures in a grid, a grid of logos and players from Bilibili, YouTube or
NetEase Cloud Music are written as HTML in a post, which needs
`markdown.unsafeHTML: true` in `kite.yaml`. Leave a blank line around any
Markdown inside:

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

### Plugins

Almanac leaves search and comments to Kite's official plugins. With
[Search](https://github.com/kite-plus/plugin-search) on the site, a search box
appears in the header and opens the plugin's search of every post. With
[Comments](https://github.com/kite-plus/plugin-comments), the thread goes under
a post, drawn in the theme's colors; a post leaves it out with
`comments: false`.

## Developing it

[`example/`](example) is a Kite site that uses the theme through a link,
`themes/almanac` to the root of this repository:

```sh
cd example
kite run
```

The stylesheet is Tailwind CSS, compiled ahead of time into
`static/almanac/almanac.css`, which is committed, so a site needs no Node.
After changing a class in a template or anything in `src/`:

```sh
npm install
npm run build
```

A change is ready when both of these pass:

```sh
kite theme verify .
(cd example && kite build --verify)
```

## Releasing

`scripts/package.sh` runs `kite theme pack`, which needs Kite 0.1.5 or
later, to pack `dist/almanac-<version>.zip`: one folder named `almanac`
holding `theme.yaml` and what the theme is made of, which the studio installs
as it is. Set `KITE` to use another kite binary than the one on `PATH`. Tag
the release and attach the zip.

## Design

The theme's pages, settings and look, and what Kite has to add for the rest of
it, are in [docs/design/README.md](docs/design/README.md).

## License

[MIT](LICENSE).
