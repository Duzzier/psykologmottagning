# CLAUDE.md

## What this is

Public website for Ivan Wahlund, legitimerad psykolog (licensed psychologist in
Sweden), offering online individual therapy. Site language is Swedish (`lang="sv"`).
The audience is prospective patients who are often anxious, in crisis, or comparing
providers. They are here to find out what treatment is, what it costs, and whether
this person is qualified — not to be impressed.

The site is regulated healthcare marketing. Under patientsäkerhetslagen (2010:659)
care must follow "vetenskap och beprövad erfarenhet", and marketing must be
*sakligt, korrekt och relevant*. Protected professional titles carry legal weight.
Treat every factual claim on this site as something that must survive scrutiny by a
patient, a colleague, or IVO.

## Commands

```bash
python3 -m http.server 8420   # serve locally; open http://localhost:8420
```

There is no build step, no package manager, no bundler, no framework, no tests.
Files are served exactly as they sit on disk. Do not introduce a toolchain.

## Structure

- `index.html` — the whole single-page site (hero, om-mig, arbetssätt, områden,
  praktiskt, FAQ, kontakt). ~730 lines.
- `faq.html` — long-form FAQ.
- One page per topic: `ocd.html`, `depression.html`, `gad.html`, `paniksyndrom.html`,
  `social-fobi.html`, `emetofobi.html`, `perfektionism.html`, `sjalvkansla.html`,
  `sorg.html`, `utmattningssyndrom.html`, `kbt.html`. Each ~400 lines and
  structurally identical: hero → symptom list → treatment description → CTA.
- `styles.css` — **currently orphaned. No HTML file links it.** Every page carries
  its own inline `<style>` block instead. Before editing `styles.css`, check whether
  the change actually needs to go into the inline blocks. Do not "fix" this by
  linking the file without confirming the two versions match — they have drifted
  (e.g. `--max` is 700px in `styles.css`, 800px in `index.html`).

When changing shared design tokens or nav/footer markup, the change has to be
repeated across all 13 HTML files. Verify with grep afterwards rather than assuming.

## Design system — already decided, do not re-invent

Tokens live in `:root` at the top of each page's `<style>`. Use the existing
variables; do not add new colors or introduce hardcoded hex values.

- Palette: warm cream/ink/olive/clay. `--cream #F5EDE9`, `--ink #2B2220`,
  `--olive #7A6A4E`, `--gold #B97B6C`, `--dark #241C1A`.
- Type: Fraunces (serif) for headings, Inter for body. No third typeface.
- Radius is deliberately near-square: `--radius-sm: 2px`, `--radius-md: 6px`.
- Layout is prose-first with a measured column (`--max`), not a grid of cards.

## Do not make this look vibe-coded

These are the empirically most common tells of AI-generated sites. None of them
belong here:

- Purple/indigo/violet anything. Gradient text. Glow, neon, or aurora effects.
- A row of three identical feature cards. Bento grids. Glassmorphism.
- Emoji used as icons or bullets, in markup or copy.
- Soft drop shadows on everything; uniform pill-shaped buttons.
- Generic marketing filler: "Transform your life", "Din resa börjar här",
  "Ta steget idag", hero copy that could describe any business.
- Decorative animation. The only motion in the codebase is a reading-progress bar
  and a 22px hero parallax, both gated on `prefers-reduced-motion`. Keep it that way.
- Stat blocks with invented numbers ("500+ nöjda klienter", "98% förbättring").
  Never fabricate outcome figures for a healthcare site.

The one `linear-gradient` in the codebase (`index.html:82`) is a photo scrim for
text legibility. That is the acceptable use.

## Content rules

The point of this site is to inform, not to persuade.

- Be concrete. "10 sessioner à 45 minuter" beats "ett skräddarsytt upplägg".
  Prices, session length, waiting time, cancellation terms, and what happens at the
  first appointment should be findable, not implied.
- Every clinical claim must be traceable to established evidence (KBT/ERP and
  related methods). Describe what a method involves and what it is used for.
  Do not promise outcomes, cure rates, or timeframes for recovery.
- Never invent credentials, affiliations, licence numbers, employment history,
  prices, contact details, or testimonials. If a fact is needed and unknown, leave a
  clearly marked placeholder and say so — do not fill the gap plausibly.
- Symptom lists describe; they do not diagnose. Keep the framing "det här kan vara
  tecken på", never "du har".
- Include crisis routing where distress content appears (112, 1177, psykiatrisk
  akutmottagning). This site is not for emergencies and should say so.

## Writing style (Swedish copy)

- Plain Swedish, "du"-tilltal, present tense, short sentences.
- No superlatives, no exclamation marks, no rhetorical questions as headings.
- No em-dash-heavy rhythm, no tricolons ("inte bara X, utan Y"), no
  "Det handlar inte om X. Det handlar om Y." constructions. These read as generated.
- Headings name the content: "Vad kostar det?" not "Investering i dig själv".
- Match the existing voice in `index.html` §om-mig: measured, first-person,
  specific about method, no emotional selling.

## Technical rules

- Semantic HTML: `<section>`, `<h2>`, `<details>/<summary>` for FAQ. Keep heading
  order unbroken; one `<h1>` per page.
- Vanilla JS only, in a single inline `<script>` at the end of `<body>`. No
  libraries, no CDN scripts, no analytics or trackers without being asked —
  this is health-adjacent traffic.
- Accessibility is non-negotiable: visible `:focus-visible` outlines (already
  styled with `--gold`), real alt text, ≥4.5:1 contrast, working keyboard nav,
  `prefers-reduced-motion` honoured by any new motion.
- Every page needs an accurate `<title>` and `<meta name="description">` written for
  the page, not templated.
- Breakpoints are ad hoc (720px nav, 700px footer, plus several one-off widths in
  `index.html`). Reuse an existing breakpoint rather than adding another.
- The contact form uses `mailto:` (`index.html:646`). It is fragile and drops
  submissions from users without a mail client. Flag this if touching contact flow;
  do not swap in a third-party form service without asking.

## Working agreements

- Make the change that was asked for. Do not redesign sections, add "improvements",
  or restructure copy that was not part of the request.
- When editing shared markup, state which of the 13 files you changed.
- Ask before: changing prices or clinical claims, adding external dependencies,
  changing the contact mechanism, or altering the colour/type system.
