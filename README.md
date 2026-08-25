# ahmednadar.com

Source for my personal site: writing on Rails, AI agents, and civic tech, plus project pages.

Live at **[ahmednadar.com](https://ahmednadar.com)**.

## Run it locally

```bash
bundle install
bundle exec jekyll serve
```

Then open http://localhost:4000. Built with Ruby 3.3.3 and Jekyll 4.4.

## Layout

| Path | What lives there |
|------|------------------|
| `_posts/` | Blog posts, `YYYY-MM-DD-slug.md` |
| `_projects/` | Project pages, output at `/projects/:title/` |
| `_layouts/`, `_includes/` | Templates |
| `assets/` | CSS, images, fonts |
| `_config.yml` | Site config, permalinks, plugins |

Plugins: `jekyll-feed`, `jekyll-seo-tag`, `jekyll-sitemap`.

## Deploys

Push to `master`. GitHub Pages builds and serves it.

One gotcha: Pages builds with its own pinned Jekyll, not the version in the
`Gemfile`. If something renders locally but not in production, that mismatch is
the first place to look.

## Use

Code is free to borrow. The writing and images are mine, please do not republish
them as your own.
