# Starter

A bare Jekyll site: maths (via KaTeX) and the two layout patterns we
worked out are wired in; everything else is deliberately undecorated.
Styling comes later — this is just something to write in.

## What's here

```
_config.yml          site settings
_layouts/default.html   the only layout — loads main.css + KaTeX
_posts/               your posts go here (filename: YYYY-MM-DD-title.md)
assets/css/main.css    the eq-grid + sidenote patterns, and nothing else
index.md              lists posts
```

## Preview locally (optional but recommended)

You need Ruby and Bundler. Then, from this folder:

```
bundle install
bundle exec jekyll serve
```

Open `http://localhost:4000`. Leave it running — it rebuilds on save.

If you'd rather skip local Ruby entirely, you can just push to GitHub
and let GitHub Pages build it (see below) — you'll only lose the fast
local preview loop.

## Publish on GitHub Pages

1. Create a GitHub repo and push this folder to it.
2. In the repo: **Settings → Pages → Build and deployment → Source**,
   choose "Deploy from a branch", and pick `main` / `(root)`.
3. GitHub builds it automatically on every push — no Actions workflow
   needed, since this uses the same `github-pages` gem GitHub itself
   builds with.
4. Your site appears at `https://<username>.github.io/<repo>/` (or at
   the root `https://<username>.github.io/` if the repo is named
   exactly that).

## Writing a post

Add a file to `_posts/`, named `YYYY-MM-DD-some-title.md`, with front
matter:

```
---
layout: default
title: "Your title"
---

Your content here.
```

## The one gotcha to know before you hit it

Kramdown (Jekyll's markdown processor) will try to parse ordinary
markdown syntax — underscores, asterisks, backslashes — even inside
blocks of raw HTML you paste in, and that collides with LaTeX syntax
(`x_1`, `a^2`, `\frac{}{}`). Fix: add `markdown="0"` to the outer
`<div>` of any HTML block containing maths, e.g.:

```html
<div class="eq-grid" markdown="0">
  ...
</div>
```

That tells kramdown "leave this alone, verbatim." See
`_posts/2026-09-18-welcome.md` for a worked example. Maths delimiters
in use are `\( ... \)` for inline and `\[ ... \]` / `$$ ... $$` for
display — deliberately not bare `$...$`, which can collide with
ordinary prose mentioning money.

## Reference

- KaTeX supported commands (flat list, not a textbook): https://katex.org/docs/supported.html
