# Dileep Guglavath — Portfolio

Personal site for [Dileep Guglavath](https://www.linkedin.com/in/dileepguglavath),
Software Development Engineer at Zapcom Solutions, Hyderabad.

One HTML file. No build step, no framework, no `npm install`.
three.js, GSAP and Lenis are the only dependencies, and all three load from a CDN.

**Live:** https://dileep-git09.github.io

---

## Structure

```
.
├── index.html                  Everything: markup, styles, content, 3D scene
├── assets/
│   ├── me.jpg                  Headshot, 720×720
│   ├── office.jpg              Office photo, 720×900
│   └── Dileep_Guglavath_Resume.pdf   ← add this yourself
├── .github/workflows/
│   └── deploy.yml              Auto-deploys to GitHub Pages on every push
├── .nojekyll                   Stops GitHub running Jekyll over the files
├── .gitignore
└── README.md
```

---

## Editing the content

Everything the site displays comes from a single `SITE` object near the top of
the `<script>` block in `index.html`. Nothing below the `RENDERING` banner
needs to change.

| Key | Controls |
|---|---|
| `stats` | The four counters under the hero. They animate up on scroll. |
| `logos` | The scrolling logo strip. `s` is the [devicon](https://devicon.dev) slug. |
| `scene` | The 3D hero: `nodes`, `edges`, and the `path` the pulse travels. |
| `experience` | Job cards. |
| `projects` | Project cards, filter chips, architecture strips. |
| `latency`, `testing`, `radar` | The three charts. Edit the numbers; the SVG rescales. |
| `path` | The education timeline. `hi: true` highlights a dot. |
| `achievements`, `certifications`, `writing` | Simple card lists. |
| `beyond`, `languages`, `toolkit` | Hobbies, languages, skills. |

### Adding a project

Append to the `projects` array:

```js
{
  name:"", kind:"", year:"",
  tags:["Backend"],            // creates a filter chip if it's new
  status:"live",               // "live" | "wip" | "nda" | ""
  flow:["client","API","db"],  // draws the little architecture strip
  blurb:"",
  stack:["Node.js"],
  details:[{h:"", p:""}],      // optional, collapses behind "How it works"
  links:[{label:"Source", url:"", key:true}]   // key:true = filled button
}
```

### Things that happen automatically

- Filter chips are derived from every `tags` value across all projects.
- **A section with an empty array hides itself**, along with its nav link.
  `writing: []` is empty on purpose — add one entry and the section appears.
- A `Person` JSON-LD block for search engines is generated from `SITE`.

---

## Running it locally

Open `index.html` in a browser. That's it.

If you want a local server so paths behave exactly like production:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

---

## Deploying

The workflow in `.github/workflows/deploy.yml` publishes the site on every
push to `main`.

**One-time setup:** in the repo, go to **Settings → Pages** and set
**Source** to **GitHub Actions**.

After that:

```bash
git add .
git commit -m "Update projects"
git push
```

Watch it deploy under the **Actions** tab. Roughly a minute.

### First push

```bash
git remote add origin https://github.com/Dileep-git09/Dileep-git09.github.io.git
git branch -M main
git push -u origin main
```

Naming the repo `Dileep-git09.github.io` is what gives you the clean
`https://dileep-git09.github.io` URL. Any other name works too, but the URL
becomes `https://dileep-git09.github.io/repo-name/`.

### Custom domain

Buy the domain, add a file called `CNAME` at the repo root containing just
your domain (for example `dileepguglavath.com`), point an ALIAS/A record at
GitHub's IPs, then set the domain under **Settings → Pages**.

---

## Premium interactions

Added on top of the original layout, all as progressive enhancement:

| Feature | What it does |
|---|---|
| Light/dark toggle | The circle button in the nav. Defaults to the visitor's OS preference (light if none), remembers the choice in `localStorage`, and live-updates the 3D hero's colors too — not just a page reload. |
| Loader | Brief branded intro on first paint, skipped instantly under reduced motion. |
| Lenis smooth scroll | Eases native scrolling; every existing scroll listener (nav, reveals) keeps working untouched. |
| Custom cursor | Dot + lagging ring, desktop only (`pointer:fine`), expands over links/cards/buttons. |
| Magnetic buttons/chips | `.btn` and `.chip` pull toward the cursor (GSAP `quickTo`), spring back on leave. |
| 3D tilt cards | Project and hobby cards tilt toward the cursor with a soft specular highlight. |
| Hero 3D scene | The node graph now pulses its halos, drifts with mouse parallax (independent of drag-to-rotate), dollies subtly on scroll, and sits inside a glowing wireframe icosahedron and additive-blended sparkle field. |
| Staggered reveals | Grid items (project cards, toolkit columns, hobbies, achievements/certs/writing rows, charts) cascade in on scroll instead of all at once. |
| Ambient gradient mesh | Slow-drifting blurred color blobs behind the hero for depth. |

## Graceful degradation

The site is built to fail quietly rather than break:

- No WebGL → the 3D canvas hides, the rest of the page is unaffected.
- GSAP/Lenis CDN blocked → loader, smooth scroll, cursor, magnetic buttons and the hero intro
  animation simply don't run; every element still renders in its final, visible state.
- A renamed devicon slug → that one logo hides instead of showing a broken image.
- `prefers-reduced-motion` → rotation, pulses, parallax, cursor lag, count-ups, the loader and
  reveals all stop or skip straight to their end state.
- Touch / coarse pointer → custom cursor, magnetic buttons and tilt cards disable themselves.
- No `IntersectionObserver` → everything renders visible immediately.
