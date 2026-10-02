# Miguel's Blog

A Hugo-powered blog published at <https://blog.matosmiguel.org>.

## Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (the version pinned in
  `.github/workflows/pages.yml`; currently `0.147.9`)
- Git

No theme download or Node.js dependencies are required. The site’s templates
and styles are maintained in this repository.

## Preview locally

From the repository root, run:

```sh
hugo server
```

Open the local URL printed by Hugo (usually <http://localhost:1313/>). Draft
posts are hidden by default; preview them with `hugo server -D`.

## Write a dated post

Create a page bundle under `content/posts/`, using the publication date and a
short, lowercase, hyphen-separated slug:

```text
content/posts/YYYY-MM-DD-post-slug/index.md
```

For example, `content/posts/2026-10-02-first-post/index.md`. Start with this
front matter and replace the title and body:

```toml
+++
title = "A post title"
date = 2026-10-02T09:00:00-07:00
draft = true
+++

Write the post here using Markdown.
```

Set `draft` to `false` when it is ready to publish. The date in front matter
controls the displayed date and chronological ordering; use the intended
publication time and timezone. Commit the post to `main` to publish it.

The home page shows the newest five published posts. The **Archive** link
opens the complete chronological post listing. Each post has its own page.

## Images and other page assets

For assets used by one post, keep them alongside its `index.md` in the page
bundle, for example:

```text
content/posts/2026-10-02-first-post/
├── index.md
└── photo.jpg
```

Reference a bundled image in Markdown as `![Description](photo.jpg)`. For
shared files such as the site stylesheet or a logo, put them under `static/`;
Hugo copies those files to the matching path in the generated site. For
example, `static/images/logo.svg` is served at `/images/logo.svg`.

## Build

Run a production build locally with:

```sh
hugo --minify
```

The generated website is written to `public/`. Hugo’s configuration, content,
layouts, and styles are in `hugo.toml`, `content/`, `layouts/`, and `static/`.

## Deploy to GitHub Pages

The workflow in `.github/workflows/pages.yml` builds with the pinned Hugo
Extended version, packages the generated site as a Pages artifact, and deploys
it through GitHub’s supported Pages deployment actions. A push to `main`
publishes the site; pull requests run the build without deploying. The
repository-root `CNAME` is copied into the artifact as `public/CNAME`, so
GitHub Pages retains `blog.matosmiguel.org` after deployment.

One-time repository setup:

1. In **Settings → Pages → Build and deployment**, select **GitHub Actions**
   as the source.
2. Keep the custom domain set to `blog.matosmiguel.org` (the committed
   `CNAME` file also configures it in each deployment).

The workflow declares the required Pages and OIDC permissions and uses the
`github-pages` environment. No separate deploy token or external theme setup
is needed. A `static/.nojekyll` file is copied into every build so GitHub
Pages never falls back to processing this repository with Jekyll.

### Troubleshooting: the site shows the README instead of the blog

This happens when **Settings → Pages → Build and deployment** is set to
**Deploy from a branch** instead of **GitHub Actions**. In that mode GitHub
ignores the Hugo output from this workflow, runs its own Jekyll build against
the raw repository contents, and — finding no `index.html` at the repository
root — renders `README.md` as the home page instead. Switching the source
back to **GitHub Actions** and re-running the latest successful `Build and
deploy to GitHub Pages` workflow run resolves it; the README is never part of
the Hugo build (it lives outside `content/` and nothing in `layouts/`
references it).
