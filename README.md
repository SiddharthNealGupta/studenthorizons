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

Light and dark are driven by `data-theme` on `<html>`. The toggle sits in the
masthead; the choice is stored in `localStorage` under `sh-theme`. First-time
visitors get whatever their OS prefers. An inline script in `<head>` sets the
attribute before first paint, so there is no flash of the wrong palette —
**leave that script where it is.**

Both palettes are defined once, as custom properties in `:root` and
`[data-theme="dark"]`. Change colours there, never inline.

Two constraints the theming has to respect:

- **Horizon Gold never sets text.** It measures 2.4:1 on white, which fails
  WCAG for reading. It draws sun arcs, step numerals, and card accents only.
- **Horizon Red splits into two tokens.** `--red-fill` stays exactly `#B92A2A`
  in both themes, because it only ever sits behind white text (6.1:1 either
  way). `--red-ink` is red used *as* text, and dark mode lightens it to
  `#DE6660` — full-strength Horizon Red on a dark ground is 3.1:1 and fails.

Every text/background pair on the page was measured in both themes; all pass
WCAG AA.

---

## The typographic rule

Every section heading carries exactly one Libre Baskerville bold-italic word in
Horizon Red. This mirrors the wordmark, where "Student" is DM Sans and
"Horizons" is Baskerville italic. Wrap that word in `<em>`; the CSS does the
rest. **One word per heading** — the effect dies if it is two.

The masthead and footer wordmarks are live text using this same rule, not
images, so they recolour with the theme.

---

## Editing common things

**The interest form link** appears in six places. Find and replace
`https://forms.gle/mtEmmoPCQnDrMjLR6` across `index.html`.

**Socials** appear in four places: the hero row, the contact tiles, the footer,
and the `sameAs` array in the JSON-LD block in `<head>`. Update all four.

**The support curve** — the four phases descend on a decaying curve
(`6.4 · 4.3 · 2.2 · 0rem`) because support is heaviest on arrival and tapers to
a steady baseline. It's the sun settling toward the horizon, not falling off
it. If you add a phase, keep the decay non-linear and update
`grid-template-columns` to match.

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
specializations, team structures, certification and Credly badges, the
ambassador directory, forums, webinars, student matching, the paid tier, other
campuses.

Still outstanding:

- **No privacy policy page.** The interest form and the contact form both
  collect personal information. Publish one before the first intake.
- **501(c)(3) is pending** and the site says so. No donation asks and no claim
  of tax-deductibility until it is granted.
