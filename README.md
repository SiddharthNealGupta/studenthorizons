# studenthorizons.org

The public site for **Student Horizons**, an independent nonprofit built by and
for students, pairing new Northeastern students with Student Ambassadors.

Static site. No build step, no dependencies, no framework. Edit the HTML and
push — that is the whole workflow.

---

## ⚠️ The contact form must be activated before it will deliver anything

The form posts to **FormSubmit**. FormSubmit does not forward mail to an
address until that address has been confirmed once, and **it silently discards
the first submission.** That is why the test message never arrived.

To activate:

1. Submit the form once on the live site.
2. Check `studenthorizons.org@gmail.com`, **including the spam folder**, for a
   FormSubmit confirmation email.
3. Click the activation link.
4. Submit once more to confirm it lands.

Until step 3 is done, FormSubmit returns `success: "false"` and the page shows
an error telling the visitor to email directly — so nothing is lost silently,
but nothing is delivered either.

Once activated, FormSubmit gives you a hashed endpoint. Swap it into the
`ENDPOINT` constant near the bottom of `index.html` so the inbox address is not
sitting in public HTML.

The form submits over `fetch` and never leaves the page: it swaps to a spinner,
then to a thank-you panel with a **Contact Us Again** button that swaps back.

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
- **The carousel** (`#join`) advances via arrows, dots, clicking the card,
  arrow keys, or swipe. Inactive slides are `inert` so they stay out of the tab
  order.
- **Click cards** (`.flip`) bulge and glow on hover to signal they are
  clickable; the fill only runs on click, and they also respond to Enter/Space
  and to tap.
- **The cursor** is a red dot with a trailing ring. Over a red-filling control,
  a second black ring is clipped every frame to that control's rectangle — so
  the arc inside the fill reads black while the arc outside stays red. Over
  non-filling targets (fields, cards, links) the ring takes a red wash instead.
  All of it disables under `prefers-reduced-motion` and on touch devices.
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
and the `sameAs` array in the JSON-LD block in `<head>`. Update all four.

**Carousel steps** — add an `<li class="car-card">`; the dots build themselves
from the card count, so nothing else needs touching.

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
