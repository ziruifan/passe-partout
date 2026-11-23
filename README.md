# passe-partout

An image-first Hugo theme for a personal gallery homepage, with light and dark modes
and English/Chinese out of the box. Requires Hugo ≥ 0.153.

- **Home**: exactly one screen tall, filled by one big tile per category. Up to 2 categories share one row, 3–4 use two columns, 5 or more use three; rows that can't be full go on top, so the bigger tiles lead (3 → 1 + 2, 5 → 2 + 3, 7 → 2 + 2 + 3, 8 → 2 + 3 + 3). Phones stack the tiles.
- **Category**: a justified photo grid. Rows fill the width and every image keeps its own aspect ratio. Hovering a photo zooms it slightly. Optional subcategories are listed in a dropdown under the category's name in the navigation; each has its own page, and switching between them on the category page is instant.
- **Tags**: every tag has a page with its photos from all categories, and `/tags/` lists them all.
- **Photo**: one image per page with prev/next (←/→ keys, swipe), a counter, a back link (or Esc), and a caption: category · subcategory, title, year, tags, an optional description, and an optional detail text. Prev/next and the counter follow the gallery the photo was opened from: the category, one subcategory, or a tag page. Going back returns to that gallery at the same scroll position. The neighbouring photos are preloaded, so stepping is instant. Above phone width the first screen is the photo alone, with a small "Details ⌄" cue at the bottom that scrolls down to the caption (and fades as the page scrolls); when the photo's shape leaves room for the whole caption under it (a landscape photo on a portrait tablet or a phone), the caption shows at the bottom of the screen instead, with the photo centred above it. The caption has three columns on wide screens (title block, description, details), two on medium screens (title block with the details under it, beside the description; without a description, the details take its place) and stacks on phones (details under the title, a thin line, then the description).
- **About**: the bio on the left and the links on the right; photos below, each shown whole at its own shape: one fills the width; several are split into rows like the homepage tiles (3 → 1 + 2, 4 → 2 + 2, 5 → 2 + 3), each row filling the width at one height (stacked on phones).
- **Notes**: long texts that photo descriptions link to, for explanations too long for a description. They stay out of the navigation, the homepage and the galleries, and list the photos that link to them.
- **Breadcrumbs**: pages have no big heading; the header shows where you are after the site name instead ("Lorem Ipsum · Lorem · Eiusmod", "Lorem Ipsum · Tags · Consectetur").
- **Page height**: every page is at least one screen tall, with the footer at the bottom; on photo pages the photo fills the first screen (see Photo).
- **Motion**: photos fade in from their placeholder colour as they load; opening a photo grows its thumbnail into the full image (and closing shrinks it back), stepping between photos morphs one into the next, switching subcategory slides the photos into place, and menus slide in. Uses View Transitions where the browser supports them, and is turned off for visitors who prefer reduced motion.
- **Light and dark**: picked automatically from the visitor's browser/OS setting (`prefers-color-scheme`). Colours are CSS variables at the top of `assets/css/main.css`.
- **Multilingual**: the language menu works like the category dropdowns (hover or click, works without JavaScript) and hides itself when the site has only one language.
- **Menus**: an open dropdown extends the top bar downwards across the full width, with its items in a row under the name.
- **Folding navigation**: whenever the name, breadcrumbs and navigation don't fit comfortably on one row (at any width, depending on the page), the navigation folds behind a menu button; it opens as a panel under the header laid out as a table, each category (and the language menu) with all its items in a row beside it.
- Images are resized to responsive WebP automatically, and phone photos are rotated the right way up.

## Example site

`exampleSite/` is a complete demo with Lorem Ipsum text (the Chinese pages use the
千字文, the traditional Chinese filler text). It has 36 generated photos (landscapes, city, water, night) in four
categories (each split into subcategories), with tags shared across categories, descriptions and an About page, in English and Chinese.
See it live at **https://ziruifan.github.io/passe-partout/** (deployed by
`.github/workflows/pages.yml` on every push to `main`), or preview it from the theme folder:

```
hugo server --source exampleSite
```

To start your own site from it, copy `exampleSite/hugo.toml` and `exampleSite/content/`
into your site and delete the `themesDir` line from the config.

## Site config

These settings have to be in your site's `hugo.toml`, because a theme's config can't
set them. `exampleSite/hugo.toml` already has them:

```toml
theme = 'passe-partout'

# Tag pages. This also turns off Hugo's default `categories` taxonomy, which the
# theme doesn't use (categories are folders, see below).
[taxonomies]
  tag = 'tags'

# Every page exists in every language (see "Translations without gaps").
[[module.mounts]]
  source = 'content'
  target = 'content'
  [module.mounts.sites.matrix]
    languages = ['**']
```

## Categories

Each category is a folder under `content/`. Pick which ones appear and in what order in `hugo.toml`:

```toml
[params]
  categories = ['lorem', 'ipsum', 'dolor', 'amet']
```

To add a category, create `content/<name>/_index.md` (plus `_index.zh.md` for its
Chinese title) and add `<name>` to the list. Without `categories`, every top-level
section is shown, ordered by `weight` in its `_index.md`. A section left off the list
still builds and is reachable by URL. It is just not linked from the homepage or nav.

Category `_index.md` fields: `title`, `weight` (only used without `categories`). The
body, if any, is shown above the gallery.

### Subcategories

A folder with an `_index.md` inside a category is a subcategory. Put its photos in it:

```
content/lorem/eiusmod/_index.md       # title, weight (menu order)
content/lorem/eiusmod/_index.zh.md    # Chinese title
content/lorem/eiusmod/ridges-dusk/    # a photo
```

In the navigation, a category with subcategories becomes a dropdown listing them.
The category's name still links to the category page; the arrow next to it opens
the menu (with a mouse, hovering opens it too). The category page shows
all of its photos. Each subcategory also has its own page (`/lorem/eiusmod/`): the
same gallery showing only that subcategory, with its name in the breadcrumbs
("Lorem Ipsum · Lorem · Eiusmod"), so it can be linked to. On the category's own pages, the
dropdown switches the gallery in place. Photos can also sit directly in the
category folder (they appear only on the category page itself, not on any subcategory page).

## Tags

Tags are shared across categories. Each tag gets a page (`/tags/<tag>/`) with all its
photos, and the tags on a photo page link there. Notes can have tags too; a tag page
lists its notes under the photos. `/tags/` lists every tag with the
number of photos and notes using it; it isn't linked from the navigation, so add it to a menu if you want
it there. Translate tags in `index.zh.md`; the Chinese site gets its own tag pages.

## Content layout

```
content/
  _index.md                 # homepage (title only)
  about/
    index.md                # layout: about; body = bio; links; photos: [a.jpg, b.jpg] (optional order)
    ridges-rose.jpg         # one or more photos (without `photos`, all images by file name)
    sea-mist.jpg
  notes/                    # long texts linked from descriptions (see "Notes")
    _index.md               # hides the folders; gives the notes the note layout
    dolorem-ipsum/          # optional category: _index.md (+ .zh.md) with its title
      ridges/
        index.md            # title, date, tags; body = the text
        ridges-left.jpg     # images used in the text
  lorem/                    # one folder per category, listed in params.categories
    _index.md               # title
    _index.zh.md            # Chinese title
    cover.jpg               # optional homepage tile image
    eiusmod/                # optional subcategory
      _index.md             # title, weight
      _index.zh.md          # Chinese title
      ridges-dusk/          # one folder per photo
        index.md            # title, date, tags, optional year/weight/featured; body = description
        index.zh.md         # optional translation
        ridges-dusk.jpg     # the photo (any file name)
```

Create a new photo with:

```
hugo new content --kind photo lorem/eiusmod/my-photo
```

Then drop the image into the new folder and set `draft: false`.

### Photo front matter

| key        | meaning                                                                 |
|------------|-------------------------------------------------------------------------|
| `title`    | Caption title                                                           |
| `date`     | Sort order (newest first) and default year                              |
| `year`     | Optional. Shown instead of the year from `date`                         |
| `tags`     | List of tags, e.g. `[Consectetur, Adipiscing]`. Each links to its tag page (see "Tags"). Translate them in `index.zh.md` |
| `weight`   | Optional manual order (lower first)                                     |
| `featured` | Optional. Use this photo as the category's homepage tile (if no `cover.*`)|
| `image`    | Optional. Which image is the photo, if the folder holds several (images used in the description never are) |
| `detail`   | Optional. Plain text on the right of the caption, line breaks kept (see "Detail") |
| `focus`    | Optional. The point to keep in view where the photo is cropped (see "Focal point") |
| `tint`     | Optional. Colour shown while the photo loads, e.g. `'#4d404d'` (see "Placeholder colour") |

The page body is the description. Leave it empty and the description block is left out.

### Images in descriptions

A description can include images. Put them in the photo's folder and use them in
Markdown; the optional title becomes a caption:

```markdown
Text before the image.

![The light behind the ridges](detail.jpg "A caption under the image")

Text after it.
```

They are resized to responsive WebP like every other image and load lazily. An image
used in the description (in any language) is never picked as the photo itself, so no
`image:` line is needed. The photo keeps its size and the page scrolls to the rest of
a long description. The same works in the About bio, plain pages and notes;
the example site's note has one.

### Detail

`detail` in a photo's front matter is plain text shown on the right of the caption
(under the title on phones), with its line breaks kept. Use it for anything:
place, camera, print size, materials.

```yaml
detail: |
  Lisbon, Portugal
  Fujifilm X-T5 · 23mm
```

To give a whole category the same detail, set it with `cascade` in the category's
`_index.md`; a photo's own `detail` still wins:

```yaml
---
title: Ipsum
cascade:
  params:
    detail: Ut enim ad minim veniam
---
```

In the example site, Lorem and Dolor photos have their own `detail`; Ipsum and Amet use
the `cascade` above.

### Notes

For explanations too long for a description, write a note and link to it. Notes live
in `content/notes/`, one folder per note (so a note can have its own images). Group
notes into categories with folders, as with photos: a category folder's `_index.md`
holds its title (and `_index.zh.md` the translation), shown above the title of each
note in it. Notes can also sit directly in `content/notes/`, without a category.

```
content/notes/
  _index.md                 # see below
  dolorem-ipsum/            # a category
    _index.md               # title: Dolorem ipsum
    _index.zh.md            # title: 寒来暑往
    ridges/                 # a note
      index.md
      index.zh.md
      ridges-left.jpg
```

`content/notes/_index.md` gives every note the note layout and keeps the folders
themselves out of the navigation, the homepage and every list (none of them becomes a
page):

```yaml
---
title: Notes
build:
  render: never        # no page for content/notes/
  list: never
cascade:
  - target:
      kind: page
    layout: note       # every note
  - target:
      kind: section
    build:             # no page for the category folders
      render: never
      list: never
---
```

Link to a note from any description with an ordinary Markdown link to its folder;
Hugo resolves it to the note in the page's language:

```markdown
[Read the full story](/notes/dolorem-ipsum/ridges)
```

A note is laid out like a photo page without the photo: the same header (back link
and language; Esc goes back too), then the caption's three columns with the category,
title, year and tags on the left, the text in the middle (headings, lists, quotes and
images as in descriptions) and, on the right, the photos whose text links to it. The
back link names that photo when there is exactly one. The example site's first Lorem
photo links to a note.

Note front matter: `title`, `date` (its year is shown) and optional `tags`, shown
below the year as on photo pages:

```yaml
---
title: Sed ut perspiciatis
date: 2025-01-01
tags: [Consectetur, Magna]
---
```

### Homepage tile image

The theme picks the first of these that exists: `cover.*` in the category folder, then a photo marked `featured: true`, then the first photo.

### Placeholder colour

While a photo loads, its box shows the photo's own dominant colour, worked out from
the image at build time, so the gallery, homepage tiles, photo pages, notes, About
photos and description images fill in with a colour that matches each photo. Set
`tint` in a photo's front matter to choose the colour yourself (one language's file
is enough). Images whose colour Hugo can't read (SVG, GIF) show a neutral grey.

### Focal point

The homepage tiles and the photo thumbnails on notes crop
photos to fill their box, and the box's shape follows the screen. By default the
crop keeps the middle. `focus` names the point to keep in view instead, as a CSS
position: `"30% 60%"` (30% from the left, 60% from the top), `"50% 20%"`, `"top"`,
`"left bottom"`. It belongs to the image, so setting it in one language's file is
enough. Galleries, photo pages and the About page always show the whole photo.

```yaml
# a photo's index.md: used on its homepage tile and on notes
focus: "50% 70%"

# a category's _index.md: for its cover.* image (overrides the photo's focus)
focus: "top"
```

## Site params

```toml
[params]
  categories = ['lorem', 'ipsum']   # categories on the homepage and in the nav, in order
  author = 'Your Name'   # header + footer (defaults to site title)
  logo = ''              # optional footer logo, path in assets/ or static/
  copyright = ''         # optional, replaces "© <year> <author>"
  description = ''       # fallback meta description
  googleFonts = true     # load web fonts from Google Fonts
  photoCounter = true    # the "3 / 9" counter on photo pages; false hides it
```

Per-language values (a translated site name, for example) go under
`[languages.zh]` and `[languages.zh.params]`. The bar shows "Language" (translated through
the `language` key in `i18n/`), and the menu lists each language by its `languageName`,
so use the full name:

```toml
[languages.en]
  languageName = 'English'
```

## Favicon

The theme ships a plain `static/favicon.ico`. To use your own, add any of these to
your site (the example site has all three):

| file                          | used by                                   |
|-------------------------------|-------------------------------------------|
| `static/favicon.ico`          | every browser (16, 32 and 48 px inside)   |
| `assets/favicon.svg`          | modern browsers, sharp at any size        |
| `assets/apple-touch-icon.png` | iPhone/iPad home screen (180 × 180, no transparency) |

The SVG and Apple icons are linked automatically when they exist.

## Fonts

Latin text uses **Ubuntu**. Pages in a `zh*` language pair it with **Noto Sans SC**,
the web version of Noto Sans CJK SC. A locally installed Noto Sans CJK SC is used first when available.
Google Fonts splits Noto Sans SC into unicode-range slices, so a page downloads only the characters it shows.
With `googleFonts = false`, the CSS falls back to local fonts only.

## Translations without gaps

The site config mounts `content/` into every language (see "Site config"). A photo
without an `index.zh.md` still appears on the Chinese site, using its English text.

UI strings live in `i18n/en.toml` and `i18n/zh.toml`.

## License

[MIT](LICENSE) © 2026 Fan Zirui
