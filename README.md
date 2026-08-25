# studenthorizons.org

The public site for **Student Horizons**, a student-run nonprofit pairing new
Northeastern University students with trained peer ambassadors through the
milestones of the move into college.

Static site. No build step, no dependencies, no framework. Edit the HTML and
push — that is the whole workflow.

---

## Deploying

The site is served by GitHub Pages from the `main` branch, root folder.

1. **Settings → Pages** → Source: *Deploy from a branch* → `main` / `/ (root)`.
2. **Settings → Pages → Custom domain** → `studenthorizons.org` → Save.
   A `CNAME` file is already committed, so this field should populate itself.
3. **Namecheap → Advanced DNS.** Delete the default parking record first, then:

   | Type  | Host | Value                        |
   |-------|------|------------------------------|
   | A     | `@`  | `185.199.108.153`            |
   | A     | `@`  | `185.199.109.153`            |
   | A     | `@`  | `185.199.110.153`            |
   | A     | `@`  | `185.199.111.153`            |
   | CNAME | `www`| `siddharthnealgupta.github.io` |

4. Wait for DNS to propagate (usually under an hour, up to 24h), then tick
   **Enforce HTTPS** in Settings → Pages.

`.nojekyll` is present so GitHub serves the files as-is rather than running
them through Jekyll.

---

## Structure

```
index.html              the entire site — one page, anchored sections
404.html                error page
assets/css/styles.css   all styling, with the brand tokens at the top
assets/fonts/           DM Sans Variable + Libre Baskerville Bold Italic (OFL 1.1)
assets/img/             logo variants, social share card, icons
CNAME                   custom domain for GitHub Pages
robots.txt, sitemap.xml
```

---

## Brand rules encoded in the CSS

Defined once as custom properties in `:root`. Change them there, not inline.

| Token           | Value     | Use                                          |
|-----------------|-----------|----------------------------------------------|
| `--paper`       | `#FFFFFF` | page background                              |
| `--ink`         | `#000000` | body and heading text                        |
| `--red`         | `#B92A2A` | Horizon Red — links, buttons, accent words   |
| `--gold`        | `#D49E2A` | Horizon Gold — **graphics only, never text** |
| `--night`       | `#121110` | footer ground                                |

Two rules worth keeping:

- **Horizon Gold never sets text.** It measures 2.4:1 against white, which
  fails WCAG for reading. It draws the sun arcs, the step numerals on a
  hairline, and the chip borders — nothing anyone has to read at small size.
- **Every section heading carries exactly one Libre Baskerville bold-italic
  word in Horizon Red.** This mirrors the wordmark, where "Student" is DM Sans
  and "Horizons" is Baskerville italic. Wrap that word in `<em>`; the CSS
  handles the rest. One word per heading — the effect dies if it is two.

Logo variants in `assets/img/` are crops and recolours of the approved
combomark. No path data was redrawn, so geometry stays identical to source.
The light variant only flips the "Student" fill to white for the dark footer.

---

## Editing common things

**The ambassador form link** appears in six places. Find and replace
`https://forms.gle/mtEmmoPCQnDrMjLR6` across `index.html`.

**A new specialization** — add an `<li>` inside `ul.chips`.

**A new FAQ** — copy an existing `<details>` block. Order is display order.

**The milestone rise** — the six stops lift by an accelerating amount
(`0 · 1.1 · 2.6 · 4.5 · 6.8 · 9.6rem`) so the row traces a curve rather than a
ramp. If you add or remove a stop, update both `grid-template-columns` and the
`nth-child` margins, and keep the increments non-linear.

---

## Accessibility floor

Do not regress these: single `h1`, alt text on every image, visible keyboard
focus, `prefers-reduced-motion` respected, no horizontal scroll at 320px, and
scroll-reveal applied via JS so content stays visible if scripts fail.

---

## Content status

The site describes the **Launch phase only** — one chapter, Northeastern.
Deliberately absent, because they belong to later phases: Credly badges, the
public ambassador directory, forums, webinars, the paid tier, other campuses.

Also outstanding:

- No student sign-up form yet; that section routes to email and Instagram.
- No privacy policy page. The FAQ carries a short plain-language answer as a
  stopgap. Publish a real policy before the first student intake.
- 501(c)(3) is pending and the site says so. Do not add donation asks or claim
  tax-deductibility until it is granted.
