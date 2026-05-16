# Blog — Plan & Theme Rearchitecture Brief

## TLDR

The blog is cross-cutting infrastructure: the single public surface where
the three pillars (music, games, LLM foundations) get written about. It is
not a fourth pillar and not part of any one pillar's execution project —
hence its own project.

The theme rearchitecture is complete and built. The theme has been renamed
from "sam" to "axiom"; its signature section-break device is
pillar-adaptive; the palette is a neutral base ground carrying four
per-pillar accents, restrained in light mode and glowing in dark. The build
is done, tested, and deployed — the blog is live at `shankars.quest`,
served from GitHub Pages and rebuilt by GitHub Actions on every push. What
remains before launch is the content itself: the lean, balanced four-post
set.

---

## Purpose

A working journal, published, spanning all three pillars plus reflective
writing that fits none of them. Honest progress, real decisions, occasional
dead ends — not a how-to and not a portfolio.

It sits outside the pillar structure deliberately. The meta planner project
holds cross-pillar planning; each pillar has its own execution project; the
blog is the cross-pillar *output* surface and has its own slot, by the same
logic that separates the planner from the execution projects.

---

## Part 1 — Infrastructure decisions (settled)

**Domain.** Two registered:
- `shankars.quest` — the primary domain; reads as "Shankar's Quest", which
  frames the blog as a journey across the three pillars. Cheap renewal on
  Porkbun. Now live, serving the blog over HTTPS.
- `shankararunachalam.com` — previously owned, still listed on the GitHub
  profile; DNS had lapsed. Reclaimed. Solves the name-commonness problem by
  using the full name. To be pointed at the blog as a redirect to the
  primary domain (see Part 5).

**Registrar.** Porkbun. (Moved off GoDaddy on pricing.)

**Hosting.** GitHub Pages, under the existing GitHub Pro account — now
live. The GitHub Actions workflow builds Hugo on the Actions runner, not
locally, and redeploys on every push to `main`.

**Repository structure.** One repository — the blog — holds everything: the
Hugo site and the `axiom` theme together, with the theme at
`themes/axiom/`. The theme was first planned as a repository of its own;
keeping it inside the blog repo is the better call in practice. `axiom` is
a bespoke theme for this one site, not a distributable product, so the
usual reason to separate it does not apply. A single repo means one history
and one `git push` — editing the theme and updating the blog are the same
act, and the Actions build picks up everything in one checkout. A separate
theme repository, pulled in as a submodule, would *add* a sync step rather
than remove one: the blog would pin a specific theme commit, and theme
changes would have to be promoted to it deliberately. That split is worth
making only if the theme later needs an independent life — reuse on another
site, its own release cycle, or public sharing. Until then, one repo.

**Writing surface.** `github.dev` — press `.` on the repository for a full
in-browser VS Code. Keeps writing possible from any machine with no local
install required. Local Hugo is used only for theme development.

**Generator.** Hugo.

**Prior site.** `sysdesign.dev` — Jekyll + Lanyon theme, GoatCounter
analytics, twelve system-design posts from 2021, dormant. Not migrated.
Treated as a closed archive; the new blog starts fresh.

---

## Part 2 — Theme: starting point and what carries forward

The rearchitecture started from a complete, working Hugo theme — formerly
"sam", now "axiom" — built and tested locally and documented in its own
README. The theme code lives in the blog repository, at `themes/axiom/`
(see Part 1, repository structure), not in this project's knowledge base;
this doc is the planning layer above that code.

**Carried forward unchanged** — the parts of the original theme that were
always sound and are explicitly kept:

- Single-column editorial layout with a disciplined vertical rhythm.
- Light/dark mode with a three-state toggle (auto/light/dark), CSS-variable
  driven.
- Pillar taxonomy plus homepage pillar cards — the multi-pillar backbone.
- Reading time, RSS, sitemap, 404, the standard furniture.
- Restraint as a constraint: one hand-written CSS file, no framework, no
  build step beyond Hugo itself.
- Typography: Fraunces + EB Garamond + JetBrains Mono. Kept as-is —
  editorial serif is pillar-neutral enough, and JetBrains Mono already
  carries the technical register. The decision is now conscious rather than
  inherited.

**The problem it had — now addressed.** As delivered, "sam" was
mono-pillar: its identity was entirely Carnatic — the name ("sam" is the
Carnatic downbeat), the signature section-break device (tala notation), and
incidental copy. Phase 1 (Part 3) corrected that. One distinction held
throughout: only reader-facing elements were ever in scope. Internal
vocabulary no reader sees — CSS variable names like `--akshara` and
`--avartana`, the trikalam-derived heading-scale ratios — is harmless
author's-craft and stays.

---

## Part 3 — Theme rearchitecture: decisions made

### Goal

A theme fully representative of all three pillars. The success test: a
reader landing on an LLM post or a game-dev post should feel the theme
belongs to that post as much as it does to the music posts. No pillar
should feel like a guest in another pillar's house.

### The unifying concept

The through-line is **foundations / primitives / first principles**: all
three pillars are about building systems up from foundational primitives.
The Carnatic instrument is built from tala and anga primitives; the game
engine from C++ data structures; the LLM work is micrograd built from
`Value` objects upward.

One framing carries this into the design. Each pillar has a *primitive
dimension* — the substrate it builds in:

- **music** is built in **time** — a rhythmic cycle.
- **games** are built in **space** — a world on a grid.
- **llm** work is built as **flow** — computation forward, gradients back.

The signature device gives each pillar a section break that is a one-row
slice of its primitive dimension. A literal shared atom holds the set
together: the dot `·` — a rest in tala, a floor tile on a map, spacing in a
computation row, the whole of the neutral journal break.

### The name — "axiom"

The theme is renamed "sam" → "axiom". "sam" was the Carnatic downbeat —
mono-pillar by construction. "axiom" is pillar-neutral and embodies the
through-line: an axiom is the irreducible primitive a system is *derived*
from, and each pillar has its axiom in exactly that sense (`Value`; the C++
data structures; tala and anga). The name also describes the theme's own
method — the signature device is one invariant backbone with everything
derived from it.

The rename touches the theme folder, `theme.toml`, the site configuration
(`theme = "axiom"`), and the README. Nothing reader-facing depends on it.

### The signature device — pillar-adaptive section breaks

The section break is the theme's signature, and it is **pillar-adaptive**:
it renders in a vocabulary drawn from the post's `pillars` front-matter
field, so every pillar gets the recognition-rewarding quality the original
tala break had, rather than that quality belonging to music alone.

Two orthogonal axes resolve the break:

1. **Pillar** (automatic, from front matter) selects the *vocabulary
   family*.
2. **Variant** (optional shortcode argument) selects the *pattern within
   that family*, defaulting per pillar.

**The four vocabularies** (default variant shown):

- **music** — Carnatic tala notation: `X · · · | O · | O ·`
- **games** — a roguelike-style quest-loop map slice:
  `· · @ · · D · · $ · >` (hero, dragon, hoard, the way onward).
- **llm** — a computation-graph slice, the forward pass meeting the
  backward pass at the gradient: `○ → ○ → ∇ ← ○ ← ○`
- **journal** — the bare shared atom: `·   ·   ·`

**The variant axis.** Each family carries a small, finite set of genuine
domain objects — the quality that made the original adi/rupaka tala tweak
feel earned rather than decorative:

- **music** — the talas: adi (default), rupaka, and the rest of the set.
- **games** — map archetypes: corridor, quest (default), lair, town.
- **llm** — the passes: forward, backward, cycle (default).
- **journal** — kept deliberately minimal: plain (default), dinkus.

**Design discipline.** Every variant, in every pillar, obeys one backbone:
a centred, monospace, light-grey row, one glyph per position. The
single-glyph rule is why the games "lone hero" row was chosen over a party
row — it keeps "one glyph per position" a real, visible rule across the
whole set. Row length varies by variant — adi is eight positions, rupaka
six — exactly as a tala's length varies; the backbone is the glyph
discipline, not a fixed width.

**The shortcode.** The original `{{< tala >}}` becomes `{{< break >}}` —
renamed for correctness, since it no longer renders only talas. It resolves
a two-level lookup, `breaks[pillar][variant]`: pillar from front matter,
variant from the optional argument or the per-pillar default, falling back
to the pillar default if a variant that doesn't belong to the page's pillar
is passed. One partial, a small nested map — no build step, still inside
the restraint budget.

**The hovertext.** Each break carries a label naming its specific pillar
and variant — "Adi tala", "the backward pass", "Quest — a roguelike map
slice". The label is keyed to the same `breaks[pillar][variant]` lookup and
surfaces as a small caption that fades in on hover. It is the answer key to
the device's recognition game — the break stays a quiet reward, but a
curious reader who doesn't know the notation is never shut out.

### The coherence ceiling

The explicit limit on per-pillar identity depth: the **accent colour** and
the **adaptive section break** are the *entire* per-pillar identity budget
in structural furniture. No per-pillar fonts, layouts, or navigation. Two
coordinated signals are enough to make a pillar feel at home; more would
make the site read as several sites.

### The base palette

The base ground is neutral. The original warm manuscript-and-saffron
palette was dropped on two grounds: it tilted the base toward music, and
saffron itself carries a political signal the blog should not wear. The
base is now a quiet, near-neutral paper in light mode and a neutral
near-black in dark mode — warmth dialled almost out. The base carries no
pillar lean; all hue identity lives in the per-pillar accents.

The four accents are **peers** — equal in visual weight, differing only in
hue, so no pillar reads louder than another:

- **music — indigo.** Replaces saffron without losing the cultural thread:
  indigo is deeply Indian (the dye, the craft) yet carries none of
  saffron's political charge, and it is the synthwave sibling of the music
  pillar's own aesthetic.
- **games — cyan.** The spatial colour — HUDs, grids, starfields — and the
  far pole of the synthwave palette from indigo. Clear of any flag-green
  reading.
- **llm — wine.** A deep red-violet; the warm counterweight to the two cool
  accents.
- **journal — neutral grey.** The neutral pillar; its accent is the absence
  of hue, by design.

A standing constraint: the palette avoids the saffron/green political
binary entirely. This rules out the *direct* signals — a true
saffron-orange, a flag-green — not warm tones or blue-greens as a category.

**Dual character.** The accents behave as one palette in two lighting
conditions. In light mode they are deep and restrained — neon does not read
on paper, and accent-coloured links must stay legible. In dark mode they
are pushed to a synthwave glow, luminous against the near-black, where that
aesthetic belongs. The journal grey stays neutral in both; glow is
something only the hued pillars do.

Working values, set as CSS variables in the theme:

- ground — light `#F6F5F2`, dark `#191816`
- music indigo — light `#4B45A6`, dark `#7E6BF2`
- games cyan — light `#0C7689`, dark `#34E0DC`
- llm wine — light `#9A3354`, dark `#E85F98`
- journal grey — light `#77756F`, dark `#9E9C92`

### The masthead

The site title is **Shankar's Quest** — singular, matching the primary
domain `shankars.quest`, so the address and the name reinforce each other.
The singular framing also says one journey across the three pillars rather
than three separate ones, consistent with the unifying concept above; and
it is warmer and truer to a working journal than the flat alternative of
the full name.

It appears verbatim in the site header. In the `<title>` tag it is
per-page: just "Shankar's Quest" on the homepage, and "Post Title —
Shankar's Quest" on posts, so every browser tab, bookmark, and search
result is self-describing. The theme assembles this automatically.

---

## Part 4 — Launch content

Balanced launch content is what makes the rearchitecture visible: a visitor
experiences the posts, not the CSS, so the pillar-adaptive break and the
per-pillar accents only register when there is a post in each pillar to
carry them.

All three pillars are at the same early stage. The "music-heavy" skew noted
earlier was a theme artifact — demo posts and a "currently" line written
when the theme was the mono-pillar Carnatic "sam" — not a real content
backlog. So balance is not performed against a lopsided reality; the launch
simply is balanced.

**The launch is four posts:**

- An **opening post** — journal category. Why the blog exists, the three
  pillars, the working-journal ethos. It frames the site, competes with no
  pillar, and puts the journal break to work from post one.
- A **music kickoff** — the Carnatic track: the shape of what is being
  taken on, and the first real step.
- A **games kickoff** — the engine and its RPG / space / cyberpunk
  direction, early-days detail and all (the project is not yet named — a
  working journal can simply say so).
- An **llm kickoff** — micrograd from scratch, built up from `Value`
  objects: the plan and the first step taken.

One intro plus a kickoff each — a clean 1-1-1. Lean by intent: at launch,
symmetry across the pillars is worth more than volume, and the journal
grows on its own afterwards. No second post anywhere at launch.

This also clears the two flagged items. The music-skewed example posts are
simply replaced by these four real ones (if the theme's example site keeps
demo content for previewing, rebalance it to one sample per pillar). And
the homepage "currently" line, with all three pillars just starting, names
all three — e.g. "currently getting three tracks off the ground".

---

## Part 5 — Build and launch

Phase 1 settled the design; this records the build that followed and what
is left before launch.

**The theme build — done.** "sam" was rebuilt into "axiom" exactly as
Part 3 specified: renamed throughout; the neutral palette and four
dual-character accents applied as CSS variables; the signature device built
as the `{{< break >}}` shortcode over a `breaks[pillar][variant]` data
file. All of it stayed inside the restraint budget — CSS variables and
small template changes, no framework, no new build step. The rebuilt theme
was verified with a Hugo build before hand-off.

**Build adjustments.** Three things were corrected once the theme was
running — two latent bugs inherited from "sam", and one revision of a
Phase 1 decision:
- *The pillar taxonomy.* The pillar is carried in a front-matter field
  named `pillars` (plural). Hugo populates a taxonomy from its plural key,
  and the singular `pillar:` the original theme used silently failed to
  generate the pillar archive pages.
- *Dark-mode code blocks.* Syntax highlighting was switched to class-based
  output so code follows light and dark mode; the inherited inline-style
  highlighting was light-only.
- *The section-break hovertext.* Phase 1 specified a native `title`
  attribute for zero CSS cost; in practice a browser tooltip was too slow
  and unstyled to register as part of the theme, so it became a small
  styled caption that fades in on hover.

**Hosting and domain — done.** The blog is a single GitHub repository (site
plus the `axiom` theme — see Part 1). A GitHub Actions workflow builds Hugo
on the runner and deploys to GitHub Pages on every push to `main`. The
custom domain `shankars.quest` is connected and serving over HTTPS. The
site is live.

**Analytics.** GoatCounter is wired into the theme as an optional,
config-driven script — it emits nothing until the `goatcounter` parameter
is set in the site config. Privacy-light, free hosted tier, nothing extra
to run.

**Development workflow.** A `CLAUDE.md` at the blog repository root carries
persistent project context — the layout, conventions, and the build and
deploy facts — so further development (theme tweaks, writing) can be done
with Claude Code starting informed rather than cold. This `plan-blog.md`
can sit in the repo alongside it for the full background.

**What remains.**

- The launch content — the four posts of Part 4. This is the last thing
  before launch.
- Set the GoatCounter `goatcounter` parameter to a real site code (sign up
  on the free hosted tier).
- Point `shankararunachalam.com` at the blog. GitHub Pages serves one
  custom domain per site, so the second domain reaches the blog via a
  Porkbun URL redirect to `shankars.quest`, not its own DNS records.
- *Optional.* Add a one-line pointer in the meta planner's
  `plan-overview.md` noting the blog now has its own project, so the
  planner stays purely about the pillars.

---

## Resources

- Live site: `shankars.quest`.
- GitHub: `github.com/shankararunachalam` (Pro account; GitHub Pages host).
  The blog repository holds both the site and the `axiom` theme.
- Domains: `shankars.quest` (primary, live) and `shankararunachalam.com`
  (reclaimed) — registrar Porkbun.
- Theme: `axiom` (formerly "sam"), inside the blog repository at
  `themes/axiom/`.
- Generator: Hugo, built and deployed by GitHub Actions.
- Writing surface: `github.dev` (press `.` on the repo).
- Development context: `CLAUDE.md` at the repository root, for Claude Code.
- Prior site (closed archive): `sysdesign.dev`.
