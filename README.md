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
  (`lectures/week01-history-of-chemical-ecology/_story1-pheromones.qmd`,
  etc.) and is pulled into the full lecture with `{{< include ... >}}`.
  Reuse a story in a different week, or a different year, by including
  it there too — edit the source once, every lecture that includes it
  updates.
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
    └── week01-history-of-chemical-ecology/
        ├── index.qmd            # the actual lecture (composes partials below)
        ├── _intro.qmd           # shared framing slides
        ├── _story1-pheromones.qmd
        ├── _story2-hipv.qmd
        ├── _story3-flower-deception.qmd
        ├── _synthesis.qmd
        └── images/              # pictures used by this week's slides
```

Each week gets its own folder under `lectures/`, named
`weekNN-<topic>` (zero-padded so they sort correctly: `week01`,
`week02`, ... `week10`). That keeps every week's slides, partials, and
images self-contained and makes it obvious at a glance which week
something belongs to.

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
quarto preview lectures/week01-history-of-chemical-ecology/index.qmd
```

**Render the whole site** (writes to `docs/`, ready to publish):

```bash
quarto render
```

**Speaker notes on a second screen while presenting:** press **`S`**
during the slideshow to pop out a Speaker View window — current slide,
next slide, your `::: {.notes}` content, and a timer. Drag that window
to your laptop screen and keep the main window fullscreen on the
projector (use *extended*, not *mirrored*, display mode). Works best
served via `quarto preview` rather than opening the rendered HTML file
directly — some browsers restrict the window-sync messaging on `file://`
URLs. Allow popups for the page if the browser blocks the window.

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

## Publishing to GitHub Pages

The site publishes automatically via GitHub Actions
(`.github/workflows/publish.yml`): every merge to `main` renders the
site with Quarto and deploys it to Pages. You never need to run
`quarto render` and commit the output — `docs/` is git-ignored and only
exists as a CI build artifact.

`main` is protected: you can't push to it directly, only merge a
reviewed pull request. So publishing a new week means opening a PR (see
below) and merging it.

## Adding a new week

1. Branch off `main`:
   ```bash
   git checkout main
   git pull
   git checkout -b week02-<topic>
   ```
2. Create `lectures/week02-<topic>/index.qmd` with its own YAML front
   matter (`format: revealjs`, etc. — copy the header from
   `lectures/week01-history-of-chemical-ecology/index.qmd` as a
   starting point), plus an `images/` subfolder if it needs pictures.
3. If a slide reuses content from an existing story (e.g. the pheromone
   story again), add `{{< include ../week01-history-of-chemical-ecology/_story1-pheromones.qmd >}}`
   there — or just copy the partial into the new week's folder if you
   want it to evolve independently from week 1's version.
4. Add the new week to the navbar in `_quarto.yml` and link it from
   `index.qmd`.
5. Preview locally (`quarto preview lectures/week02-<topic>/index.qmd`),
   then commit, push the branch, and open a pull request into `main`.
6. Merging the PR triggers the Actions workflow, which publishes the
   updated site automatically.

## Adding pictures to a slide

1. Drop the image file into that week's `images/` folder, e.g.
   `lectures/week01-history-of-chemical-ecology/images/moth-antenna.png`.
2. Reference it from the slide with a normal Markdown image, using a
   path relative to the `.qmd` file:
   ```markdown
   ![Caption text](images/moth-antenna.png)
   ```
3. To control size/placement in revealjs, use Quarto's fenced attribute
   syntax:
   ```markdown
   ![Caption text](images/moth-antenna.png){width="60%" fig-align="center"}
   ```
4. Because the deck uses `embed-resources: true`, images are embedded
   directly into the rendered HTML — the slide deck stays a single,
   portable file with no separate image files to keep track of.

## A note on the content itself

The Fabre antennae-removal bullet in Story 1 was deliberately corrected
here from a more commonly popularized (but slightly inaccurate) version:
Fabre's own account shows the antennae experiment was inconclusive by
his own admission, since intact "control" males also failed to return.
The sealed-box experiment is his cleanest, best-supported result. See the
commit history on `lectures/_story1-pheromones.qmd` for the full note.
