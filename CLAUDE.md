# CLAUDE.md

## Site structure

| File | Purpose |
|---|---|
| `index.html` | Everything — layout, styles, data, JS |
| `humans.txt` | Human-readable site credits |
| `llms.txt` | Plain-text summary for AI crawlers |
| `robots.txt` | Crawl permissions + sitemap pointer |
| `sitemap.xml` | URL index for search engines |

---

## Adding a Now/Recently entry

All entries live in the `ENTRIES` array in `index.html`. Add a new object:

```js
{
  id:      "slug-here",       // kebab-case, unique
  section: "now",             // "now" | "recently"
  question: "...",            // the question or title shown as headline
  context:  "...",            // one-paragraph answer or context
  tags:    ["tag-a"],         // short keyword strings
  date:    "Jun 2026",        // display string
  dateISO: "2026-06",         // machine-readable, used in <time datetime="...">
  link:    null,              // or "https://..." for a "read full post" link
},
```

After adding, update the companion files (see Maintenance checklist below).

---

## Adding an Archive article

Archive articles live in the `ARTICLES` array and render as a full-page view.

### Step 1 — Build the article object

```js
{
  id:       "slug-here",
  title:    "...",
  subtitle: "...",
  date:     "2026",           // display string
  dateISO:  "2026-01-01",     // machine-readable (UTC), used for sort/filter — use the real month/day if known, else Jan 1
  section:  "archive",
  tags:     ["tag-a", "tag-b"],
  image:    null,             // or { src: "path/to/img.jpg", alt: "..." }
  body:     `...HTML...`,
}
```

The Archive sidebar list is sorted by `dateISO` (newest first), so this field must always be set.

### Body conventions

| Content type  | HTML |
|---|---|
| Paragraph     | `<p>text</p>` |
| Section header | `<h2>Title</h2>` |
| Pull quote    | `<blockquote>quote</blockquote>` |
| Stat callout  | `<div class="stat-line"><span><strong>77%</strong> label</span></div>` |
| Inline code   | `<code>snippet</code>` |

Do not wrap `body` in a `<div>` — it is injected directly into `.article-body`.

### Step 2 — No separate index entry needed

Archive items are rendered automatically in the sidebar from the `ARTICLES` array. No HTML changes required.

---

## Maintenance checklist

Run through this whenever content is added, removed, or updated:

### `llms.txt`
- Add new `ENTRIES` items to the relevant section (Now / Recently closed)
- Add new `ARTICLES` items to the Writing section
- Remove entries that have been removed from the site
- Keep answers/summaries current — this is what AI crawlers read

### `humans.txt`
- Update `Last update:` date

### `sitemap.xml`
- Update `<lastmod>` to today's date

### `index.html` — JSON-LD
- `knowsAbout` array: add any new topic areas that appear in new entries

### `index.html` — meta description
- If the site's focus shifts, update the `<meta name="description">` in `<head>`

---

## Hidden / easter egg entry

The `ENTRIES` array contains one entry with `hidden: true` (id: `"konami"`). It is excluded from the sidebar and prev/next navigation — only reachable via the Konami code (↑↑↓↓←→←→ba). Do not remove the `hidden: true` flag or it will appear in the public list.

---

## Keyboard shortcuts

| Key | Action |
|---|---|
| `j` | Next entry (older) |
| `k` | Previous entry (newer) |
| ↑↑↓↓←→←→ba | Reveal hidden entry |
