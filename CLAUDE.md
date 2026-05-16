# Shankar's Quest — blog

A published working journal spanning three project threads ("pillars"):
Carnatic music, an RPG game engine, and LLM foundations. Honest progress
notes, not a how-to or a portfolio. Live at https://shankars.quest.

A Hugo static site. The custom theme, `axiom`, lives in `themes/axiom/`
inside this repo — not a separate package.

See @plan-blog.md for full background and design rationale.

## Repo layout

- `hugo.toml` — site configuration
- `content/` — pages and posts; posts go in `content/posts/`
- `static/` — assets copied verbatim; `static/CNAME` holds the domain
- `themes/axiom/` — the theme: templates, one CSS file, `data/breaks.yaml`
- `.github/workflows/hugo.yaml` — builds with Hugo, deploys to GitHub Pages

## Commands

- `hugo server` — local preview on :1313
- `hugo server -D` — preview including drafts
- `hugo --gc --minify` — production build (what CI runs)

Requires Hugo extended ≥ 0.140. Deploy is automatic: push to `main` →
GitHub Actions → GitHub Pages.

## Writing posts

- A post is a Markdown file in `content/posts/`. Front matter fields:
  - `pillars:` — one of `music | games | llm | journal`. Note the plural
    key: it is the taxonomy field. Singular `pillar:` will NOT work.
  - `title`, `date`, `tags`, `summary`; `draft: true` until ready.
- One pillar per post.
- For a section break, use the `{{< break >}}` shortcode. It adapts to the
  post's pillar automatically; an optional argument picks a variant.

## Theme conventions — do not break these

- `axiom` is deliberately minimal: one CSS file, no JS framework, no build
  step beyond Hugo. Keep it that way.
- A pillar's whole identity budget is two things — its accent colour and
  its section break. Do not add per-pillar fonts, layouts, or navigation.
- Palette: a neutral base plus four accents — music indigo, games cyan,
  llm wine, journal grey — each with a light and a dark value.
- Section-break glyph vocabularies live in `themes/axiom/data/breaks.yaml`.
- `hugo.toml` must keep `[markup.highlight] noClasses = false`, so syntax
  highlighting is class-based and works in dark mode.

## Deploy and domain

- `baseURL` in `hugo.toml` is `https://shankars.quest/`.
- The workflow's build step runs plain `hugo --gc --minify`. Do NOT add a
  dynamic `--baseURL` flag — on a project repo it injects a wrong subpath
  and breaks every asset URL.
- `static/CNAME` must contain exactly one line: `shankars.quest`.
