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

## Writing equation blocks without typing HTML

`_includes/mathformat.html` turns one line per equation row into the
full `eq-grid` markup, at build time (no JavaScript):

```liquid
{% include mathformat.html rows="x || = y + z || definition of x
|| = a + b + z || substituting y" %}
```

Each line is `lhs || rhs || note` — leave a field blank by putting
nothing between its pipes (e.g. an empty `lhs` on a continuation
line). See `_posts/2026-09-18-welcome.md` for it in use next to the
hand-written version it replaces.

Two things to know about it, both general Jekyll facts rather than
anything specific to this include:

- Liquid scans the *whole* raw file for `{%` and `{{`, regardless of
  surrounding HTML. If a line of maths ever contains the literal
  sequence `%}` (a stray `%` right before a `}`) or a double brace
  like `{{`, it can break the build by confusing Liquid's tag
  parsing. If that happens, wrap the offending text in
  `{% raw %} ... {% endraw %}` — Jekyll's built-in way of saying
  "don't process this bit."
- If the multi-line `rows="..."` parameter ever proves flaky in
  practice, the fallback is a small `_data/*.yml` file per equation
  set instead — more ceremony, but immune to the point above since
  YAML doesn't share Liquid's delimiter characters.

## GitHub's own math preview (and why it won't show this)

Editing a `.md` file directly on github.com has a "Preview" tab that
renders `$...$` and `$$...$$` math live, via GitHub's own Markdown
renderer (backed by MathJax). It's genuinely useful for plain inline
maths in ordinary prose — but it never runs Jekyll or Liquid, so
anything inside an HTML block, or behind `{% include mathformat.html
... %}`, is invisible to it; GitHub just shows the raw, unprocessed
text. For those, preview via `bundle exec jekyll serve` instead (see
above), ideally with `--livereload` so the browser refreshes on save.

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
