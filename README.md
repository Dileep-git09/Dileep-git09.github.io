# Dileep Guglavath — Portfolio

Personal site for [Dileep Guglavath](https://www.linkedin.com/in/dileepguglavath),
Software Development Engineer at Zapcom Solutions, Hyderabad.

One HTML file. No build step, no framework, no `npm install`.
three.js is the only dependency and it loads from a CDN.

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

## Graceful degradation

The site is built to fail quietly rather than break:

- No WebGL → the 3D canvas hides, the rest of the page is unaffected.
- A renamed devicon slug → that one logo hides instead of showing a broken image.
- `prefers-reduced-motion` → rotation, pulses, count-ups and reveals all stop.
- No `IntersectionObserver` → everything renders visible immediately.
