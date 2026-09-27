# Design System — Prince Aryan GitHub Profile

## 1. Taglines considered
- Building products that feel lighter than code. *(chosen — used as the hero typing line)*
- Engineering Intelligence Into Beautiful Interfaces.
- AI • Full Stack • Systems • Design
- Designing Digital Gravity.

## 2. Color palette

| Token | Hex | Use |
|---|---|---|
| `--bg` | `#09090B` | Base background |
| `--bg-alt` | `#0C0C10` | Secondary surface |
| `--fg` | `#FAFAFA` | Primary text |
| `--fg-muted` | `#A1A1AA` | Secondary text |
| `--border` | `#27272A` | Dividers, card edges |
| `--blue` | `#3B82F6` | Accent — full stack / links |
| `--violet` | `#8B5CF6` | Accent — AI / brand |
| `--cyan` | `#22D3EE` | Accent — terminal, active states |
| `--emerald` | `#34D399` | Accent — success, OSS |

## 3. Typography scale
- Display (hero name): 88px / 700 / -2 tracking — Inter / SF Pro Display fallback
- H1 (section titles): 28px / 700
- H2 (card titles): 18–20px / 600
- Body: 14–15px / 400, `--fg-muted`
- Mono (terminal, typing line): 14–18px, JetBrains Mono / SFMono fallback

Stack declared in-file as `Inter, 'SF Pro Display', sans-serif` for prose and `'JetBrains Mono', monospace` for code/terminal blocks, since GitHub README rendering can't load custom @font-face — these are safe system/GitHub-available fallback chains.

## 4. SVG asset specifications

| File | Purpose | Key techniques |
|---|---|---|
| `assets/svg/hero.svg` | Hero banner | Radial-gradient aurora blobs + `animateTransform` drift, particle field with `animate` on cy, animated `linearGradient` on name text, pulsing glow ring around avatar placeholder, blinking terminal cursor |
| `assets/svg/divider.svg` | Section divider | Animated `linearGradient` sweep (x1/x2 keyframes) |
| `assets/svg/footer.svg` | Footer | Two layered sine-like wave paths animated via `<animate>` on `d`, pulsing headline opacity |
| `assets/svg/timeline.svg` | Development journey timeline | Dashed connector line with `stroke-dashoffset` animation, pulsing node circles, rotating dashed "future" node |
| `assets/icons/focus-*.svg` (×5) | Current Focus glyphs | Shared radial glow + pulsing outer ring per icon, minimal line-glyph set |

All SVGs are hand-authored, dependency-free, and use only GitHub-permitted SVG animation (`animate`, `animateTransform`) — no JS, no external fonts required to render correctly (system fallbacks used).

## 5. README wireframe (top → bottom)
1. Hero banner (SVG)
2. Introduction card (table-based glass-style layout)
3. Current Focus (5-up icon grid: Studying AI/ML, Full Stack, Linux, OSS, Exams)
4. Tech Stack (categorized `<details>` sections, skillicons.dev grids — no badge spam)
5. Featured Projects (2-col card table + collapsible fundamentals archive)
6. GitHub Analytics (stats, top languages, streak)
7. Development Journey Timeline (SVG)
8. Linux Setup (ASCII terminal block)
9. Development Philosophy (quote)
10. Setup / Workflow (Editor / Terminal specs)
11. Connect With Me (icon row)
12. Footer (SVG aurora wave)

## 6. Section-by-section rationale
- **Hero**: sets the entire tone in one viewport — name, role, and motion before any scrolling, mirroring Linear/Vercel landing pages.
- **Introduction**: kept to a scannable table instead of paragraphs; recruiters get the facts in under 3 seconds.
- **Current Focus**: five equal-weight cards telegraphing breadth (Studying AI/ML + full stack + systems + open source + defence exams).
- **Tech Stack**: `<details open>` blocks replace badge walls — same information density, far less visual noise, and collapsible on repeat visits.
- **Projects**: table-based cards give every project the same shape (stack, highlights, links) with clean repository links.
- **Analytics**: external stats widgets themed to match the palette (`bg_color`, `title_color`, etc. query params) so they don't look bolted on.
- **Timeline**: puts the growth narrative (2023→2026→Future) in one horizontal read with evenly spaced non-overlapping milestones.
- **Linux setup**: signals technical depth informally — a deliberate change of pace from the polished sections above it.
- **Footer**: closes the page with the same aurora motif as the hero, bookending the design.

## 7. Animation specification
All animation is native SVG, GitHub-safe (no JS, no CSS transitions/keyframes since GitHub strips `<style>` transition rules on embedded raw markdown — animation lives inside referenced `.svg` files instead, which GitHub renders as `<img>` and *does* animate):
- `animateTransform` (translate) — aurora blob drift, 9–13s loops, eased via slow triplet values
- `animate` on `cy`/`r`/`opacity` — particle float, glow pulses, timeline node pulses
- `animate` on gradient `x1`/`x2` — name shimmer, divider sweep
- `animate` on path `d` — footer wave motion
- `stroke-dasharray` + `dashoffset` animation — timeline connector "flow" effect

## 8. Accessibility notes
- Every `<img>` tag carries descriptive `alt` text (not empty, except pure dividers which are decorative).
- Hero and footer SVGs include `role="img"` + `aria-label`.
- Color pairs (text on background) chosen for ≥ 4.5:1 contrast: `#FAFAFA` / `#A1A1AA` on `#09090B`.
- Motion is subtle and slow (4s+ loops) — no strobing, no rapid flicker, respecting vestibular-safety norms even though `prefers-reduced-motion` cannot be read inside a GitHub-rendered README.
- Structure uses semantic Markdown headings (`##`) throughout so the page remains navigable via a screen reader's heading list even inside the HTML/table layout.

## 9. Performance optimization
- All custom SVGs are hand-written (no exported bloat from design tools), keeping each file under ~4KB.
- No raster images embedded — only referenced placeholders (avatar, Spotify) that the user swaps in later.
- External widgets (stats/streak/activity/trophy) are lazy-loaded by GitHub's own image proxy (`camo.githubusercontent.com`) automatically — no action needed, but keep the widget count as-is (6) to avoid slow first paint on profile view.
- `skillicons.dev` batches multiple icons into a single request per category instead of one request per badge — this is the "no badge spam" requirement's performance payoff, not just visual.

## 10. Before publishing — replace these placeholders
- Avatar image URL in the Introduction section and hero glow circle
- University / department / semester in Introduction
- Spotify UID in the Now Playing widget (or delete that section)
- Discord user ID link
- All project descriptions/stacks marked `[...]`
- GitHub username `princearyan` throughout stats URLs → your actual username
