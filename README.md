# studenthorizons.org

The public site for **Student Horizons**, a student-run nonprofit pairing new
Northeastern University students with peer ambassadors who help them find their
footing on campus.

Static site. No build step, no dependencies, no framework. Edit the HTML and
push — that is the whole workflow.

---

## Before this goes live: activate the contact form

The contact form posts to **FormSubmit**, which needs no account but does need
one activation:

1. Publish the site.
2. Submit the form once yourself.
3. FormSubmit emails `studenthorizons.org@gmail.com` with a confirmation link.
   Click it. Submissions start arriving after that.

Until you do this, the form silently fails. Once activated, FormSubmit will
give you a hashed endpoint — swap it into the `action` attribute in
`index.html` so the inbox address isn't sitting in public HTML.

If you'd rather not depend on a third party, the alternative is embedding a
Google Form in that column, which is one `<iframe>` swap.

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

4. Wait for DNS (usually under an hour, up to 24h), then tick **Enforce HTTPS**.

**Every file must sit at the repo root.** `index.html` directly at the top
level, not inside a folder. Pages can only publish from root or `/docs`.

---

## Structure

```
index.html              the site — one page, anchored sections
thanks.html             form success page (FormSubmit redirects here)
404.html                error page
assets/css/styles.css   all styling; tokens at the top
assets/fonts/           DM Sans Variable + Libre Baskerville Bold Italic (OFL 1.1)
assets/img/             brandmark, social share card, icons
CNAME, .nojekyll, robots.txt, sitemap.xml
```

---

## Theming

**Dark is the default.** Light only appears if the visitor has explicitly
chosen it; the choice is stored in `localStorage` under `sh-theme`. An inline
script in `<head>` sets `data-theme` before first paint so there is no flash of
the wrong palette — **leave that script where it is.**

The palette is absolute. Three values, no tints, no greys, no intermediates:

| Token     | Dark      | Light     |
|-----------|-----------|-----------|
| `--paper` | `#000000` | `#FFFFFF` |
| `--ink`   | `#FFFFFF` | `#000000` |
| `--red`   | `#B92A2A` | `#B92A2A` |

Horizon Red is identical in both themes and is never lightened. It carries
every rule, border, button outline, fill, and emphasis word. Horizon Gold is
not used anywhere on this build except inside the brandmark artwork itself.

Because no third text colour is permitted, **hierarchy is made with size and
weight only.** Resist the urge to add a grey for secondary copy.

One thing to know: `#B92A2A` on `#000000` measures 3.1:1, below the WCAG AA
floor of 4.5:1 for body text. Red is therefore confined to display sizes,
rules, and fills, and never sets small reading copy — so no body text on the
page falls below the floor. If you later set a paragraph in red, it will.

---

## The typographic rule

Emphasis words are set in Libre Baskerville bold italic, in Horizon Red. This
mirrors the wordmark, where "Student" is DM Sans and "Horizons" is Baskerville
italic. Wrap the word in `<em>` and the CSS does the rest.

---

## Editing common things

**The interest form link** appears in six places. Find and replace
`https://forms.gle/mtEmmoPCQnDrMjLR6` across `index.html`.

**Socials** appear in four places: the hero row, the contact tiles, the footer,
and the `sameAs` array in the JSON-LD block in `<head>`. Update all four.

**The flip cards** (`.flip`) show face A, and slide face B up over it on
hover. They also respond to keyboard focus and to a tap or click, because
hover-only content is unreachable on touch and by keyboard — keep all three
triggers if you edit them.

**Panels** each fill one screen (`min-height: calc(100svh - var(--head))`) with
their content vertically centred, and the page uses `scroll-snap-type: y
proximity` so scrolling settles on a section. If you add content to a panel,
re-check that it still fits at 1280×800 — the tightest common laptop — or the
section will spill into the next one. Below 900px wide, snapping and fixed
heights are both switched off.

---

## Accessibility floor

Don't regress these: single `h1`, alt text on every image (decorative marks use
`alt=""` plus `aria-hidden`), visible keyboard focus, `prefers-reduced-motion`
honoured, no horizontal scroll at 320px, and scroll-reveal applied via JS so
content stays visible if scripts fail.

---

## Content status

Describes the **Launch phase only**: one chapter, Northeastern, one role.

Deliberately absent because they belong to later phases: role tiers and
specializations, team structures, Credly badges, the ambassador directory,
forums, webinars, student matching, the paid tier, other campuses.

Still outstanding:

- **No privacy policy page.** The interest form and the contact form both
  collect personal information. Publish one before the first intake.
- **501(c)(3) is pending** and the site says so. No donation asks and no claim
  of tax-deductibility until it is granted.
