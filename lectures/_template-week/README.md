Template for a new week's lecture. The leading underscore keeps this
whole folder out of the rendered site — copy it, then rename the copy
to drop the underscore.

## To use it

```bash
cp -r lectures/_template-week lectures/weekNN-topic
```

Then, in `lectures/weekNN-topic/`:

1. In `index.qmd`'s YAML front matter, set `title`, `subtitle` (French),
   and `topic-image` (a picture for this week, e.g.
   `images/your-picture.jpg`). That's the whole title slide — it's
   built automatically for every week from the shared
   `../../title-slide.html` template (logos, the "ÉCOLOGIE CHIMIQUE ·
   BSc S5" header, and the author line all come from that template plus
   the `author_Zu` value in the project's `_quarto.yml`, so you don't
   write any of that per week).
2. Work through each `TODO` in `_agenda.qmd`, `_learninggoals.qmd`,
   `_hook.qmd`, `_content.qmd`, `_checkpoint.qmd`, `_synthesis.qmd`.
   Add more content partials as needed (`_story2.qmd`, etc.) and
   `{{< include ... >}}` them from `index.qmd`.
3. Drop images into `images/` and reference them as `images/file.png`.
4. Add the week to the navbar in `_quarto.yml` and to the list in the
   top-level `index.qmd`.
5. Optional: a live progress header (one line, section name + dots that
   fill in as you present) can show on every slide except the title.
   Tag the *first* heading of each top-level part of your session with
   `data-agenda-section="Some Name"`, e.g.:
   ```markdown
   ## Welcome {background-color="#1B4332" data-agenda-section="Welcome"}
   ```
   Every slide from that heading up to the next `data-agenda-section`
   marker counts as belonging to "Some Name" - no need to tag every
   slide, just where each new part starts. See
   `lectures/week01-history-of-chemical-ecology/` for five worked
   examples. The header itself (`progress-header.html` +
   `#agenda-progress-header` in `styles/forest-theme.scss`) is already
   wired in via `include-after-body` above — it just does nothing until
   you add at least one marker.

See `lectures/week01-history-of-chemical-ecology/` for a filled-in
example of this same structure.
