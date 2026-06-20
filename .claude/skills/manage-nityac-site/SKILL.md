---
name: manage-nityac-site
description: >-
  Operating guide for nityac.com — Nitya's personal Hugo website (repo
  nityac-hugo). Use whenever the user asks anything about managing, editing,
  publishing, or understanding her website: adding or editing an essay/post,
  changing a page (about, work, connect, contact), changing the sidebar name,
  subtitle, avatar, or menu, previewing the site locally, publishing/deploying,
  fixing a broken link or URL, "where does X show up", "how do I add Y to my
  site", or any reference to nityac.com, "my website", "my Hugo site", or "my
  blog/essays". Explains what file renders what page, where things live, the
  publish pipeline, and common gotchas — written so someone with zero Hugo
  knowledge can act.
---

# Managing nityac.com (for dummies)

This is the operating manual for **nityac.com**, Nitya's personal website.
Read this first whenever she asks about her site — it tells you the structure,
what renders where, and how to make changes safely.

## The 30-second mental model

- The site is built with **Hugo** (a static site generator) using the
  **Stack theme**. Hugo is **not a CMS** — there's no dashboard. You **write
  Markdown files**, Hugo turns them into styled web pages.
- Repo: `nityachid/nityac-hugo`. Local path: `/Users/nityac/Code/nityac-hugo`.
- Live domain: **nityac.com** (set by `static/CNAME`), hosted on **GitHub Pages**.
- **Publishing is automatic:** commit + push to `main` → a GitHub Action
  (`.github/workflows/hugo.yml`) builds the site and deploys it. ~20–60s later
  it's live. No manual upload.

```
edit a .md file  →  git push to main  →  GitHub Action builds  →  live on nityac.com
```

## What renders what, and where (the key map)

Everything that becomes a page lives in `content/`. File → URL:

| File | Live URL | What it is |
|---|---|---|
| `content/_index.md` | `nityac.com/` | **Homepage.** Currently shows the "About" text (What I Do / The Journey / etc.), inside the Stack theme's sidebar layout. |
| `content/essays/_index.md` | `/essays/` | **Essays list page** — auto-lists every essay as cards. |
| `content/essays/<slug>.md` | `/essays/<slug>/` | **One essay.** e.g. `scaling-without-breaking.md` → `/essays/scaling-without-breaking/` |
| `content/about.md` | `/about/` | About page (currently "Coming soon"). |
| `content/work.md` | `/work/` | Work page (currently "Coming soon"). |
| `content/connect.md` | `/connect/` | Connect page (currently "Coming soon"). |
| `content/contact.md` | `/contact/` | Contact page (exists but NOT in the nav menu). |

**Essays are the main section.** In `config.toml`, `mainSections = ['essays']`
tells the theme that `content/essays/` is the blog feed. (It used to be called
`posts` — it was renamed to `essays` so the URL matches the "Essays" nav item.)

### Site chrome (sidebar, menu, footer) lives in `config.toml`, NOT in content:
- **Name** ("Nitya Chidambaram") = `title` at top of `config.toml`.
- **Subtitle** ("Operator. Builder. Systems thinker.") = `[params.sidebar] subtitle`.
- **Avatar photo** = `[params.sidebar.avatar] src` → points at `img/profile-thumb.jpg`
  (actual file: `static/img/profile-thumb.jpg`; `static/` is served at the site root).
- **Nav menu** (Work, Essays, About, Connect) = the `[menu]` block.
- **Footer text** = `[params.footer] customText`.

## How to do the common things

### Add a new essay
```bash
cd /Users/nityac/Code/nityac-hugo
hugo new essays/my-essay-title.md     # scaffolds the file with frontmatter
```
Then edit `content/essays/my-essay-title.md`:
- Write the body in Markdown (use `##` for section headings).
- **Set `draft: false`** (the scaffold defaults to `draft: true`, which hides it from the live build).
- Optional: fill in `description`, `tags`, `categories: ["Essays"]`.
- URL will be `/essays/my-essay-title/`.

### Edit an existing page or essay
Just open the matching `.md` file from the table above and edit the Markdown.

### Change the name / subtitle / menu / avatar
Edit `config.toml` (see "Site chrome" above). **Config changes require a server
restart** to preview — Hugo does NOT hot-reload `config.toml`.

### Preview locally before publishing
```bash
cd /Users/nityac/Code/nityac-hugo
hugo server          # then open http://localhost:1313/
```
Content edits live-reload automatically. Stop with `pkill hugo`.

### Publish (make it live)
```bash
git add -A
git commit -m "your message"
git pull --rebase origin main   # in case remote is ahead
git push origin main            # triggers the deploy Action
```
Watch the deploy: `gh run watch $(gh run list --branch main --limit 1 --json databaseId --jq '.[0].databaseId')`

## Gotchas (these will bite you — read them)

1. **Drafts are invisible.** A file with `draft: true` won't appear on the live
   site. Plain `hugo server` also hides drafts; use `hugo server -D` to preview them.
2. **`config.toml` changes need a server restart** (content changes don't).
3. **Stale `public/` folder** can make the local dev server show old/ghost pages
   (e.g. a deleted URL still returning 200). Fix: `pkill hugo; rm -rf public resources/_gen; hugo server`.
   The real test of what will deploy is a clean build: `hugo --cleanDestinationDir -d /tmp/check`.
4. **The theme is a git submodule** (`themes/hugo-theme-stack`). If `themes/` is
   empty after a fresh clone or branch switch, run `git submodule update --init --recursive`.
5. **CNAME = the domain.** Don't delete `static/CNAME` (contains `nityac.com`) or
   the custom domain breaks on the next deploy.

## History / context (so you're not confused later)

- The site was originally a Hugo blog with a sample "first-post" and a `posts`
  section. In June 2026 the section was renamed `posts` → `essays`, the sample
  post and a "Coming soon" essays placeholder were removed, and the first real
  essay ("Scaling Without Breaking") was added.
- **There is a parked alternative design.** A hand-built static HTML redesign
  (Stitch-based, "Northern Editorial" — terracotta/teal/bone, Libre Caslon +
  Plus Jakarta Sans) was built but **not** adopted — it's saved in `git stash`
  (`git stash@{0}`) along with a static-deploy workflow and the migrated files in
  an `_hugo_old/` move. Nitya may revisit or drop it. The design notes are in
  `DESIGN.md` at the repo root. **If she asks about "the new design" or "the HTML
  version," that's what she means** — check `git stash list`.
- Known small loose ends (may still exist): a leftover test file
  `essays/placeholder/index.html`, and a dead `/posts/first-post` link in
  `content/work.md`.

## Quick command cheat sheet
```bash
cd /Users/nityac/Code/nityac-hugo
hugo new essays/<slug>.md     # new essay
hugo server                   # preview at localhost:1313
hugo server -D                # preview incl. drafts
pkill hugo                    # stop preview
hugo --cleanDestinationDir -d /tmp/check   # test a real build
git add -A && git commit -m "..." && git push origin main   # publish
```
