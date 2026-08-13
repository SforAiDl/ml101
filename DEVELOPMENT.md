# Running and editing this site

The site is [Jekyll](https://jekyllrb.com/) built on the
[Just the Class](https://github.com/kevinlin1/just-the-class) template, which extends the
Just the Docs theme. GitHub Pages builds it automatically on push. Everything below is for
previewing changes locally first.

## Run it locally

You need **Ruby 3.3** and Bundler. Version matters in both directions: macOS's system Ruby
(2.6) is too old for the GitHub Pages gems, and Ruby 4.0 is too new, because `commonmarker`,
which GitHub Pages depends on, requires Ruby `< 4.0`. Ruby 3.3 is what GitHub Pages itself
builds with.

```bash
brew install ruby@3.3
echo 'export PATH="/opt/homebrew/opt/ruby@3.3/bin:$PATH"' >> ~/.zshrc
exec zsh
ruby -v   # should report 3.3.x
```

Then, from the repository root:

```bash
bundle config --local path vendor/bundle   # already set in .bundle/config
bundle install
bundle exec jekyll serve --livereload
```

Open <http://localhost:4000/just-the-class/>. The path suffix is the `baseurl` from
`_config.yml`. To preview at the bare root instead:

```bash
bundle exec jekyll serve --livereload --baseurl ''
```

`--livereload` rebuilds and refreshes the browser on every save. `_config.yml` is the one
exception: changes there require restarting the server.

To just check that the site builds without serving it:

```bash
bundle exec jekyll build
```

## Before going live

Update `url` and `baseurl` in `_config.yml` to match wherever the site is published
(see [this guide](https://mademistakes.com/mastering-jekyll/site-url-baseurl/)). For a repo
published at `https://<org>.github.io/<repo>/`, set `url: 'https://<org>.github.io'` and
`baseurl: '/<repo>'`.

## Where the content lives

| What | Where | Notes |
|:-----|:------|:------|
| Home page | `README.md` | Doubles as the repo README, uses `layout: home` |
| Announcements | `_announcements/` | One file per announcement, named `YYYY-MM-DD-slug.md`. Newest is shown first. |
| Lectures | `_modules/` | One file per unit, numbered `01-` to `05-` to control ordering. Rendered on `lectures.md`. |
| Weekly schedule | `_schedules/weekly.md` | Day and time grid. Currently a **TODO** placeholder, so fill in `schedule:` once timings are confirmed. |
| Instructors | `_staffers/` | One file per person, numbered `01-` to `12-` to control ordering. Photos live in `assets/images/`. |
| Home page | `README.md` | Course description, prerequisites, lectures, resources, and contact details. |
| Colors | `_sass/color_schemes/saidl.scss` | SAiDL purple palette, selected via `color_scheme: saidl` in `_config.yml`. This is the only styling change from the stock template. |

### Adding a lecture

Lectures are definition lists inside a unit file in `_modules/`, using the template's format:

```markdown
L13
: **New Topic**: A short summary of what the lecture covers.
  : [Slides](https://example.com)
```

### Adding an announcement

Create `_announcements/YYYY-MM-DD-some-slug.md`:

```markdown
---
title: Announcement title
week: 3
date: 2026-09-01
---

Body text in Markdown.
```

Link to other pages with an absolute path, `[lectures]({% raw %}{{ site.baseurl }}{% endraw %}/lectures/)`. Announcements are collection documents rendered inside `announcements.md`, so the relative `lectures.md` style of link used on ordinary pages does not get rewritten here and will 404.

### Adding or changing an instructor

Create a numbered file in `_staffers/` and drop the photo in `assets/images/`:

```markdown
---
name: Full Name
role: Instructor
email: todo@todo.com
website: https://example.com
photo: filename.jpg
---
```

The `role: Instructor` value is what `instructors.md` filters on, so entries with any other
role will not appear.
