# Project page — Seeing Through the Displaced Frame

Static site for the ICRA 2027 submission. One HTML file plus an `assets/` folder, no build step,
no dependencies. Total size about 30 MB, well inside GitHub Pages limits.

```
index.html
.nojekyll
assets/
    video.mp4                  18.9 MB   supplementary video, 2:48, 720p
    paper.pdf                   8.9 MB
    fig_displaced_frame.png     0.7 MB
    fig_method.png              0.3 MB
    fig_tasks.png               1.0 MB
    scene_real_peg.png          0.6 MB   video poster frame
```

---

## ⚠ Read this before publishing

**The paper is under double-anonymous review.** `main.tex` has `\ANONYMOUS=1`, and the ICRA 2027
CFP requires double-blind. The page as written contains **no names, no affiliation and no email**,
and says so on screen.

Publishing it under `<your-username>.github.io` still ties the work to your GitHub account. That
is a judgement call you need to make, not a technical one. Three options:

1. **Wait until acceptance.** Safest. Keep the repo private, publish on the camera-ready date.
2. **Publish under a fresh account** with a neutral name, e.g. `displaced-frame.github.io`, and do
   not link it from anywhere that carries your name. This is the usual practice for anonymous
   project pages during review.
3. **Publish under your own account now**, accepting that a determined reviewer could connect it.
   ICRA does not forbid preprints or project pages, but it does ask that authors not actively
   advertise their identity to reviewers.

Whichever you pick, do not add names until the paper is accepted.

---

## Publish

Create a repo and push:

```bash
cd forge-ts-project-page
git init -b main
git add .
git commit -m "Project page"
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Source: Deploy from a branch → main → / (root) → Save**.

The site appears at `https://<user>.github.io/<repo>/` within a minute or two. For a user site
instead, name the repo exactly `<user>.github.io` and it serves from the root domain.

`.nojekyll` is there so GitHub serves the files as-is rather than running them through Jekyll.

## Preview locally

```bash
python -m http.server 8731
```

Then open `http://localhost:8731`. Opening `index.html` by double-clicking also works, but some
browsers block the video over `file://`, so the server is more reliable.

---

## After acceptance

Everything that needs changing is marked with a comment in `index.html`.

**1. Authors.** In the header, delete the `<div class="anon">` block. Then in the stylesheet remove
`display:none` from `.authors` and `.affil`, and fill in the real names and affiliation:

```html
<p class="authors">Author One<sup>1</sup> &nbsp; Author Two<sup>1</sup></p>
<p class="affil"><sup>1</sup>Department, Institution</p>
```

**2. Venue line.** Change `Under review · ICRA 2027` in `<p class="venue">` to the accepted venue.

**3. Footer note.** Delete or rewrite the `<div class="note">` block, which currently explains the
anonymity.

**4. BibTeX.** Replace the anonymous entry in the `#bibtex` section.

**5. Code link.** The Code button is disabled. Point its `href` at the repo and drop
`aria-disabled="true"`.

---

## Design notes

Colour follows the paper's own key, so the page and the figures agree: **blue** (`--blue`, the
Okabe-Ito `#0072b2` the paper uses as `cbBlue`) is the student and the true pose, **vermillion**
(`--verm`, `#d55e00`) is the believed pose and the baselines. If you restyle, keep that pairing or
the figures will contradict the tables.

Type is Source Serif 4 for headings and Source Sans 3 for body, loaded from Google Fonts, with
JetBrains Mono for the BibTeX block.

Light and dark are both defined as token sets on `:root`. Dark is applied through
`@media (prefers-color-scheme: dark)` guarded by `:root:not([data-theme="light"])`, and again
under `:root[data-theme="dark"]` so an explicit choice wins either way. Every colour comes from a
token, so changing a theme means editing one block, not hunting through rules.

Verified: no horizontal overflow at 375 px or 1280 px, all images and the video load, tables scroll
inside their own container rather than pushing the page sideways, both themes meet contrast.

## Updating the video

The current video is the v6 cut, in which all six side-by-side hardware comparisons have a genuine
FORGE baseline (`actor.pth`) on the right. If you rebuild it, replace `assets/video.mp4` and check
the duration in the `#video` section heading still matches.
