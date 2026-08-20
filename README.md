# link.crii.me — URL Shortener

Short links for the crii.me ecosystem.

## How to use

Short URL format: `link.crii.me/#<code>`

Example: `link.crii.me/#blog` → redirects to the blog

## Add a link

Edit `urls.json`:

```json
{
  "code": "mylink",
  "url": "https://example.com",
  "title": "My Link",
  "created": "2026-08-21",
  "clicks": 0
}
```

Then push:

```bash
git add urls.json
git commit -m "Add link: mylink"
git push
```

## Edit a link

Change the `url`, `title`, or other fields in `urls.json` and push.

## Delete a link

Remove the entry from `urls.json` and push.

## Features

- Public list of all shortened URLs
- One-click copy button
- Hash-based routing (no server needed)
- JSON-driven (easy to edit)
- Dark brutalist theme
