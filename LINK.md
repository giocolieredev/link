# LINK.md — link.crii.me

A tiny URL shortener. Short URLs look like `link.crii.me/#blog` and redirect to the real URL. It's static (GitHub Pages) — the list of links lives in `urls.json` and the page fetches it at runtime, so **adding a link = editing one JSON file and pushing**.

---

## How it works

- `link/index.html` fetches `urls.json` from GitHub's raw CDN:
  `https://raw.githubusercontent.com/giocolieredev/link/main/urls.json`
- On load it checks `window.location.hash`. If the hash matches a `code`, it redirects: `link.crii.me/#blog` → `https://blog.crii.me`.
- The same data also renders the public list of all links on the page, each with a one-click copy button.

Because it's hash-based, there are no server redirects — the browser does the jump.

---

## Add a link

Edit `urls.json` and add an entry:

```json
{
  "code": "mylink",
  "url": "https://example.com/some/page",
  "title": "My Link",
  "created": "2026-08-22",
  "clicks": 0
}
```

- `code` — what appears in the short URL: `link.crii.me/#mylink`. Keep it short, no spaces or `#`.
- `url` — the destination (include `https://`).
- `title` — shown in the list on the page.
- `created` — date, for your own reference.
- `clicks` — reserved field, not tracked yet (see below).

Then push:

```bash
git add urls.json
git commit -m "Add link: mylink"
git push
```

Live in about a minute.

## Edit a link

Change `url`, `title`, etc. in `urls.json` and push. Old codes keep working unless you rename `code`.

## Delete a link

Remove its entry from `urls.json` and push. The short URL stops working (the page shows the list with no redirect).

---

## Files

```
link/
├── index.html     the page (renders list + handles #code redirect)
├── urls.json      ★ the only file you edit
├── CNAME          link.crii.me
├── robots.txt     points Google at sitemap.xml
├── sitemap.js     regenerates sitemap.xml (reads CNAME)
├── sitemap.xml    auto-generated
└── README.md
```

## Setup & deploy

- Repo: `github.com/giocolieredev/link`, GitHub Pages from `main` / root (`.nojekyll` present).
- `CNAME` = `link.crii.me`; DNS `link.crii.me` → `giocolieredev.github.io` (wildcard `*.crii.me` usually covers it).
- The sitemap Action (`.github/workflows/sitemap.yml`) auto-updates `sitemap.xml` on push.

---

## Notes & gotchas

- **Clicks are not counted.** The `clicks` field is reserved for future tracking — currently nothing increments it. If you want real analytics, I can add a tiny counter (e.g. a Firebase/Upstash/Worker endpoint) or you can point the codes at your normal link-analytics service.
- The page fetches `urls.json` from GitHub raw — if you rename the repo or branch, update the `JSON_URL` constant near the top of the `index.html` script.
- Codes are case-sensitive (`#Blog` ≠ `#blog`).
- Keep `code` URL-safe: lowercase letters, numbers, dashes.
- Don't name a code the same as a real page on the domain (e.g. avoid `#index`).
