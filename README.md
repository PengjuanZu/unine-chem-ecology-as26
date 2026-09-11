# Écologie Chimique — Course Repository

Source files for the Chemical Ecology (BSc S5) course, University of
Neuchâtel. Built with [Quarto](https://quarto.org); version-controlled
with Git.

## Why this setup

- **Traceable** — every edit is a Git commit. Run `git log` on any file
  to see exactly what changed, when, and why. Tag a commit `2026-edition`
  before you start revising for next year, so you can always get back to
  what you taught this year.
- **Reusable** — each story/topic lives in its own file
  (`lectures/_story1-pheromones.qmd`, etc.) and is pulled into the full
  lecture with `{{< include ... >}}`. Reuse a story in a different
  lecture, or a different year, by including it there too — edit the
  source once, every lecture that includes it updates.
- **Easy to edit** — everything is plain Markdown. No PowerPoint,
  no fighting text boxes. Speaker notes live in `::: {.notes} ... :::`
  blocks right next to the slide they belong to.
- **One source, multiple outputs** — the same `.qmd` file can render to
  revealjs slides (an interactive webpage), and Quarto can also produce
  a PDF or Beamer version from the same source if you ever want a
  printable handout.

## Project layout

```
chem-ecology-course/
├── _quarto.yml                 # site-wide config (navbar, theme)
├── index.qmd                   # course homepage
├── styles/
│   ├── forest-theme.scss       # revealjs theme (forest green / amber)
│   └── site.css                # styling for the non-slide website pages
└── lectures/
    ├── history-of-chemical-ecology.qmd   # the actual lecture (composes partials below)
    ├── _intro.qmd                        # shared framing slides
    ├── _story1-pheromones.qmd
    ├── _story2-hipv.qmd
    ├── _story3-flower-deception.qmd
    └── _synthesis.qmd
```

Files starting with `_` are **partials** — Quarto won't render them as
standalone pages, only as includes inside another file. That's what
makes them reusable building blocks.

## Local setup (one-time)

1. Install Quarto: <https://quarto.org/docs/get-started/>
2. Install Git (usually already on macOS/Linux; on Windows use
   [Git for Windows](https://git-scm.com/download/win)) — or use
   [GitHub Desktop](https://desktop.github.com/) if you prefer a GUI over
   the command line.
3. Open this folder in VS Code (with the Quarto extension) or RStudio.

## Day-to-day workflow

**Preview while editing** (auto-refreshes in your browser as you type):

```bash
quarto preview lectures/history-of-chemical-ecology.qmd
```

**Render the whole site** (writes to `docs/`, ready to publish):

```bash
quarto render
```

**Save your changes to history** (do this whenever you finish a useful
chunk of editing — a sentence, a slide, a whole story):

```bash
git add -A
git commit -m "Describe what you changed, e.g. 'Fix Fabre antennae bullet'"
```

**See what changed and when:**

```bash
git log --oneline              # list of all commits
git diff HEAD~1                # what changed in the last commit
```

**Tag an edition before revising for a new year** (so you can always
return to exactly what you taught):

```bash
git tag 2026-edition
git push --tags
```

## Publishing to GitHub Pages (free, public web link)

1. Create an empty repository on GitHub (don't initialize it with a
   README — this folder already has one).
2. From this folder:
   ```bash
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git branch -M main
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages → Source → Deploy from a branch →
   `main` / `docs`**. Save.
4. Your course site will be live at
   `https://<your-username>.github.io/<repo-name>/` within a minute or
   two. Re-run `quarto render`, commit, and push any time you want to
   update the live site.

## Adding a new lecture next semester

1. Duplicate the pattern: create `lectures/lecture2-<topic>.qmd` with its
   own YAML front matter (`format: revealjs`, etc.).
2. If it shares content with an existing story (e.g. you reuse the
   pheromone story in a different lecture), just add
   `{{< include _story1-pheromones.qmd >}}` there too.
3. Add a link to it from `index.qmd`.
4. Commit as usual.

## A note on the content itself

The Fabre antennae-removal bullet in Story 1 was deliberately corrected
here from a more commonly popularized (but slightly inaccurate) version:
Fabre's own account shows the antennae experiment was inconclusive by
his own admission, since intact "control" males also failed to return.
The sealed-box experiment is his cleanest, best-supported result. See the
commit history on `lectures/_story1-pheromones.qmd` for the full note.
