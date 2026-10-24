# bartoszgorka.com

Source of my personal blog, built with [Jekyll](https://jekyllrb.com/) and the
[Basically Basic](https://github.com/mmistakes/jekyll-theme-basically-basic) remote theme.

## Publishing

Every push to `master` is built with `actions/jekyll-build-pages` (the same builder GitHub uses for
classic Pages builds) and deployed by the [Pages workflow](.github/workflows/pages.yml).
Pull requests only build the site, so a broken article never reaches production.

## Local preview

```bash
bundle install
bundle exec jekyll serve --livereload
```

The site is then available at <http://localhost:4000>.

## Writing an article

1. Create `_posts/YYYY-MM-DD-title.md` with the front matter used by existing posts.
2. Open a pull request and fill in the template.
3. Merge once the build is green.
