# DBAeyes blog

Personal Jekyll blog of Vincent, served by GitHub Pages at https://dbaeyes.github.io
(the old `www.yyds.au` domain was retired in Oct 2026 and must not come back).
Posts are mostly Chinese, from 2007 to now.

## Writing a new post

- File: `_posts/YYYY-MM-DD-English-Slug.md` (date = publish date, slug in English, words joined by `-`).
- Front matter, copied from recent posts:

  ```yaml
  ---
  layout: post
  title: "文章标题"
  categories: [Life]
  tags: [Tag1, Tag2]
  description: 一句话简介，显示在首页列表
  author: "Vincent"
  header-img: "img/fantasy.jpg"
  permalink: /:categories/:year/:month/:day/:title.html
  ---
  ```

- `categories`: reuse an existing one — `Life`, `MyLife`, `Football`, `Daily`, `Code`, `AWS`, `Software`, `Digital`, `Thinking`.
- `header-img` is optional; pick from `img/`. Put new images for a post in `img/` and reference them as `/img/name.jpg`.
- Body is GitHub-flavoured Markdown (kramdown GFM). Keep Vincent's voice; don't pad or add a closing summary he didn't ask for.

## Rules

- Don't touch old posts (2007–2023) unless asked. Their broken hotlinked images are kept on purpose as memories.
- Work on a branch and open a PR; merge only when Vincent says so.
- Pushing to `master` deploys automatically (`.github/workflows/deploy.yml`, ~1 min). PRs have no CI, so build locally before opening one.
- The custom domain is controlled in repo Settings → Pages, not by a `CNAME` file. Don't add one.
- Repo-only files (like this one) belong in `exclude` in `_config.yml` so they aren't published.

## Local build

```sh
export LANG=C.UTF-8 LC_ALL=C.UTF-8   # without this Sass fails with "Invalid US-ASCII character"
bundle config set path vendor/bundle
bundle install
bundle exec jekyll build             # or: jekyll serve
```

Don't commit `Gemfile.lock` or `vendor/`.
