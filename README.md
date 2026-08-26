# studenthorizons.org

The public site for **Student Horizons**, an independent nonprofit built by and
for students, pairing new Northeastern students with Student Ambassadors.

Static site. No build step, no dependencies, no framework. Edit the HTML and
push — that is the whole workflow.

---

## The contact form

Posts to FormSubmit's AJAX endpoint using the activated hashed ID, so the inbox
address is not sitting in public HTML:

```
https://formsubmit.co/ajax/472aa6e980aa76fe485a71f6145cff68
```

It submits over `fetch` and never leaves the page — the panel swaps to a
spinner, then to the thank-you message with a **Contact Us Again** button that
swaps back. If the request fails, it returns to the form and shows an error
pointing the visitor at the Gmail address, so a message is never silently lost.

The success check reads the response body, not just the HTTP status:
FormSubmit answers `200 OK` with `success: "false"` in some failure states, so
status alone would report a false success.

---

## Deploying

Served by GitHub Pages from the `main` branch, root folder.

1. **Settings → Pages** → Source: *Deploy from a branch* → `main` / `/ (root)`.
2. **Settings → Pages → Custom domain** → `studenthorizons.org` → Save.
   A `CNAME` file is committed, so this should populate itself.
3. **Namecheap → Advanced DNS.** Delete the default parking record, then:

   | Type  | Host  | Value                          |
   |-------|-------|--------------------------------|
   | A     | `@`   | `185.199.108.153`              |
   | A     | `@`   | `185.199.109.153`              |
   | A     | `@`   | `185.199.110.153`              |
   | A     | `@`   | `185.199.111.153`              |
   | CNAME | `www` | `siddharthnealgupta.github.io` |

4. Wait for DNS, then tick **Enforce HTTPS**.

**Every file must sit at the repo root** — `index.html` at the top level, not
inside a folder. Pages can only publish from root or `/docs`.

---

## Structure

```
index.html              the site — one page, anchored sections
404.html                error page
assets/css/styles.css   all styling; tokens at the top
assets/fonts/           DM Sans Variable + Libre Baskerville Bold Italic (OFL 1.1)
assets/img/             wordmark + combomark in both inks, share card, icons
CNAME, .nojekyll, robots.txt, sitemap.xml
```

### Section IDs

`#home` · `#do` · `#role` · `#join` · `#why` · `#contact`

The header nav, the footer Pages column, and the section order all follow that
sequence. If you add a section, add it to all three.

---

## Theming

**Dark is the default.** Light only appears if the visitor explicitly chooses
it; the choice is stored in `localStorage` under `sh-theme`. An inline script in
`<head>` sets `data-theme` before first paint so there is no flash of the wrong
palette — **leave that script where it is.**

The palette is absolute. Three values, no tints, no greys, no intermediates:

| Token     | Dark      | Light     |
|-----------|-----------|-----------|
| `--paper` | `#000000` | `#FFFFFF` |
| `--ink`   | `#FFFFFF` | `#000000` |
| `--red`   | `#B92A2A` | `#B92A2A` |

Horizon Red is identical in both themes and never lightened. Because no third
text colour is permitted, **hierarchy is made with size and weight only** —
resist adding a grey for secondary copy.

`#B92A2A` on `#000000` is 3.1:1, under the WCAG AA floor of 4.5:1 for body
text. Red is confined to display sizes, rules and fills and never sets small
reading copy, so no body text on the page falls below the floor. Set a
paragraph in red and it will.

### Theme-paired artwork

Logos ship as two files each, one per ink, swapped by CSS:

| Slot   | Dark theme               | Light theme             |
|--------|--------------------------|-------------------------|
| Header | `wordmark-light.svg`     | `wordmark-dark.svg`     |
| Footer | `combomark-light.svg`    | `combomark-dark.svg`    |

Tag them `.on-dark` and `.on-light`; the CSS hides the wrong one. Both are the
supplied artwork with only the `viewBox` cropped to the mark — no geometry was
redrawn and no colours were altered.

---

## Interactions

- **Buttons** are hollow with a red fill that slides up on hover.

- **The theme toggle** is the mark in miniature: a Horizon Gold sun over a
  horizon line. Above the line is light (day), below is night. Each toggle adds
  another `+180deg`, so the sweep is always clockwise — light→dark passes down
  the right, dark→light continues round the left. The rotation counter is
  unbounded on purpose; **never reset it to 0**, or the next switch will spin
  backwards. The gold never changes on hover; the horizon line flips to
  `--ink-inv` so it stays legible against the red fill.

- **The carousel** (`#join`) carries a clone of the last card before the first
  and a clone of the first after the last. Stepping past either end slides onto
  the clone normally, then jumps to the matching real card with the transition
  switched off — which is why 5→1 slides one card left instead of rewinding
  through the whole track. Clicking the left half goes back, the right half
  forward; arrows, dots, arrow keys and swipe all work too. Inactive slides are
  `inert` so they stay out of the tab order.

- **Click cards** (`.flip`) and the carousel bulge and glow on hover to signal
  they are clickable; the fill only runs on click. Cards also respond to
  Enter/Space and to tap.

- **The cursor** is a red dot with a trailing ring, red in both themes. Over a
  control that fills red, dot and ring both turn black — but the colour change
  carries a `0.3s` delay so it lands *as the fill arrives*. Without that delay
  you get a black ring on a black page for the length of the fill, which reads
  as the cursor vanishing. The base rule has no delay, so leaving reverts
  instantly. Fields never switch to an I-beam: the expanded ring is the "you
  can type here" signal. Disabled under `prefers-reduced-motion` and on touch.

- **No scroll snapping.** Sections are sized by their content; scrolling is free.

---

## The typographic rule

Emphasis words are set in Libre Baskerville bold italic, in Horizon Red. This
mirrors the wordmark, where "Student" is DM Sans and "Horizons" is Baskerville
italic. Wrap the word in `<em>` and the CSS does the rest.

---

## Editing common things

**The interest form link** appears in five places. Find and replace
`https://forms.gle/mtEmmoPCQnDrMjLR6` across `index.html`.

**Socials** appear in four places: the hero grid, the contact grid, the footer,
and the `sameAs` array in the JSON-LD block in `<head>`. Update all four. The
social links are labelled **Gmail**; the contact form's own field is still
labelled **Email**, because it asks for the visitor's address and they may not
use Gmail.

**Carousel steps** — add an `<li class="car-card">`; the clones and the dots
are both built in JS from the card count, so nothing else needs touching.

**Two-line paragraphs** — the ones broken with `<br>` carry `.two`, `.two-42`
or similar, which cap the measure so the wrapped line can't outrun the forced
first line. Those selectors are deliberately **single-class and flat**: add a
descendant rule like `.face .two` and it will out-specify the modifiers and
silently undo the tuning.

---

## Accessibility floor

Don't regress these: single `h1`, alt text on every image, visible keyboard
focus, `prefers-reduced-motion` honoured, no horizontal scroll at 320px,
scroll-reveal applied via JS so content stays visible if scripts fail, and
every hover-triggered behaviour also reachable by keyboard and tap.

---

## Content status

Describes the launch phase only: one chapter, Northeastern, one role.

Deliberately absent because they belong to later phases: role tiers and
specializations, team structures, Credly badges, the ambassador directory,
forums, webinars, student matching, the paid tier, other campuses.

Still outstanding:

- **No privacy policy page.** The interest form and the contact form both
  collect personal information. Publish one before the first intake.
- **501(c)(3) is pending** and the footer says so. No donation asks and no
  claim of tax-deductibility until it is granted.

---

## Rule system

Every horizontal line on the page is one of three kinds, each defined once in
`:root` and never restated inline:

| Kind        | Count | Geometry                       | Thickness |
|-------------|-------|--------------------------------|-----------|
| Full-bleed  | 2     | edge to edge                   | `--rule`  |
| Section     | 7     | `--rule-col` (content column)  | `--rule`  |
| Title       | 5     | content column                 | `--rule`  |

The two full-bleed rules are the masthead's bottom border and the footer's top
border. The seven section rules are the six `.panel + .panel` dividers plus the
one above the footer legal line — that last one is drawn with a `::before`
rather than a border, because a border on `.foot-legal` spans the shell's
padding box and would land a gutter wider than its six peers. The five title
rules are the `.label` underlines.

Section and title rules share `--rule-col`, so every inset line on the page
starts and ends at the same two x-coordinates.

## Interaction states

`--press` and `--press-sm` set how far a control contracts on click; the two
values exist because perceived press depth is absolute, not proportional, so a
2.4rem icon button needs a larger factor than a full-width button to read as
the same press. The click cards and the carousel keep their own 1.014 → 1.004
pair, since they are an order of magnitude larger again.

Text fields carry three states that must stay distinct: **hover** is the bulge
and red radiance, **active** is the press, **focus** is the inward border and
nothing else. The focus rule is declared last so it wins while the pointer is
also over the field.

## Theme

Dark on every load. Nothing is persisted — no `localStorage`, no
`sessionStorage` — so a reload or a fresh visit always comes back dark. In-page
section links do not reload the document, so a chosen theme survives navigating
around the page.
