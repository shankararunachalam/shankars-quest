# axiom — a Hugo theme

> *quiet editorial typography for a working journal*

A minimal Hugo theme for personal blogs that span several project threads at
once. Each post belongs to a **trail**; the theme keeps the trails visually
distinct without ever letting the design fragment. The whole identity budget
per trail is two things: an accent colour and a section break.

## What makes it distinctive

- **Trail-adaptive section breaks** via the `{{< break >}}` shortcode. The
  break renders in a glyph vocabulary chosen by the post's trail — Carnatic
  tala notation for music, roguelike map glyphs for gaming, a small computation
  graph for LLM posts, a bare dot rhythm for the journal. One device, four
  dialects.
- **Per-trail accent colour.** Links, focus rings, underlines and the like
  pick up the hue of the page's trail. A music post reads in indigo, a gaming
  post in cyan, an LLM post in wine; pages with no trail stay neutral grey.
  Set once, in CSS, via a `data-trail` attribute on `<body>`.
- **Multi-trail post structure.** Posts group by trail; each trail gets an
  archive page and a homepage card, and the homepage surfaces the trails
  side-by-side as equal peers.
- **Editorial typography.** Fraunces (display, with variable optical-sizing
  and the SOFT/WONK axes), EB Garamond (body), JetBrains Mono (code).
- **Light/dark mode.** CSS variables, system preference by default, an
  explicit toggle that cycles auto → light → dark. Accents are dual-character:
  restrained in light mode, with a quiet glow in dark mode.
- **No framework, no build step** beyond Hugo itself. One CSS file.

## Installing

```bash
# In your Hugo site directory:
git submodule add https://github.com/shankararunachalam/hugo-theme-axiom themes/axiom

# Or, without submodules:
git clone https://github.com/shankararunachalam/hugo-theme-axiom themes/axiom
```

In your `hugo.toml`:

```toml
theme = "axiom"
```

Then copy the example site config to use as a starting point:

```bash
cp themes/axiom/exampleSite/hugo.toml ./hugo.toml
# customize: title, baseURL, author, trails
```

## Required configuration

The theme expects a few `params` to be set. See `exampleSite/hugo.toml` for
the canonical example. Minimal:

```toml
[params]
  description = "What your blog is about"
  author = "Your Name"
  tagline = "a quiet subtitle"
  currently = "What you're working on right now (shown on the home page)"

  # At least one trail
  [[params.trails]]
    slug = "music"          # must match the `trails` field in post front matter
    name = "Music"          # display name
    description = "Brief description of this thread"
```

Taxonomies must be configured for trails to work as filterable categories:

```toml
[taxonomies]
  trail = "trails"
  tag = "tags"
```

Code blocks use class-based syntax highlighting, so they follow light and
dark mode. Set this once:

```toml
[markup.highlight]
  noClasses = false
```

### Analytics (optional)

The theme can emit a [GoatCounter](https://www.goatcounter.com) counter — a
privacy-light, cookie-free visit count. It is off unless you set it:

```toml
[params]
  goatcounter = "https://yourcode.goatcounter.com/count"
```

Leave it unset and nothing is emitted. Analytics is wired in a single
partial, `layouts/partials/analytics.html`, if you want to swap providers.

## Writing posts

Front matter:

```yaml
---
title: "Post title"
date: 2026-05-12
trails: "music"           # music | gaming | llm | journal — must match a trail slug
tags: ["norns", "carnatic"]
summary: "Optional short description (also used as og:description)"
---
```

### The break shortcode

The signature device. Use it as a section break inside posts:

```markdown
First section text.

{{< break >}}

Next section text.
```

`{{< break >}}` is **trail-adaptive**: with no argument it renders the
default vocabulary for the post's trail. The same call produces a tala on a
music post and a computation graph on an LLM post. Hovering the break shows
the variant's name via the native `title` attribute.

To pick a specific variant, pass its name:

```markdown
{{< break "rupaka" >}}    <!-- a named music variant -->
{{< break "backward" >}}  <!-- the backward pass, an llm variant -->
{{< break "lair" >}}      <!-- a gaming variant -->
```

An unknown variant falls back to the trail's default; a post with no trail
falls back to the neutral journal break.

### The trail shortcode

Inline reference to a trail within prose:

```markdown
This connects to my {{< trail "music" >}} work on Norns.
```

Renders as a small chip linked to the trail's archive page.

## Customizing

### Colours

Edit the design tokens at the top of `assets/css/main.css`. The theme uses
CSS variables throughout — you change the tokens, not the rules.

- **Ground / ink / rule** — the neutral surfaces and text. Light and dark
  values are defined in the `:root`, `@media (prefers-color-scheme: dark)`
  and `:root[data-theme="…"]` blocks.
- **Trail accents** — `--trail-music`, `--trail-gaming`, `--trail-llm`,
  `--trail-journal`. These are the only hues in the palette.
- **`--accent`** is *contextual*: it defaults to the neutral journal hue and
  is reassigned per page by the `[data-trail="…"]` rules. `--accent-soft`
  and `--accent-faint` are derived from it with `color-mix()`, so they follow
  whichever trail and colour mode are in scope automatically.

To add a trail, add a `--trail-<slug>` variable (light and dark) and a
matching `[data-trail="<slug>"]` rule.

### Section break vocabularies

The break glyphs live in `data/breaks.yaml`, not in template code. The
shape is `breaks[trail].variants[variant]`, each variant carrying `glyphs`
and a `label`. Add a tala, a map archetype or a whole new trail's vocabulary
by editing that file — no template changes needed.

### Typography

Fonts load from Google Fonts in `layouts/partials/head.html`. To change them,
edit the `<link>` href and the `--serif-display` / `--serif-body` / `--mono`
variables in `main.css`. The Fraunces variable axes (`opsz`, `SOFT`, `WONK`)
are used throughout; if you swap the display font for one without them, drop
the `font-variation-settings` rules.

## Browser support

Modern browsers. The theme uses CSS custom properties and `color-mix()` (in
all current browsers since 2023). It degrades gracefully — typography and
layout stay sound in older browsers; you lose the derived accent shades.

## License

MIT. Use it, modify it, ship something with it.

## Acknowledgements

The music trail's section break uses standard South Indian tala notation —
the `X · | O` symbols documented in Trichy Sankaran's *The Rhythmic
Principles & Practice of South Indian Drumming* (1994). The other trails'
vocabularies (roguelike map glyphs, computation graphs) are drawn from their
own domains in the same spirit: notation borrowed, not invented.
