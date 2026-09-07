# truegilb.github.io

Gilbert's blog, built with [Jekyll](https://jekyllrb.com) and a trimmed-down version of the
[So Simple Theme](http://mademistakes.com/so-simple/).

## Local development

```
bundle install
bundle exec jekyll serve
```

Then visit `http://localhost:4000`.

## Deployment

Pushes to `master` build and deploy automatically via the GitHub Actions workflow in
`.github/workflows/pages.yml`. The site is served at `https://blog.truetsang.com` (see `CNAME`).
