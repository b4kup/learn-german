# Handover — static site for German-learning exercises

**For:** Claude Code
**Goal:** Create a public Git repo containing three static HTML pages and publish it on GitHub Pages (zero build step). Hand back the live URL.

---

## Context

I have two self-contained, single-file HTML apps (interactive German exercises for Danish speakers, A2–B2). No frameworks, no build, no bundler — each file already inlines its own CSS/JS and pulls fonts from Google Fonts via CDN. They just need to be served as static files.

A landing page (`index.html`) that links both is already written and included.

## Inputs (files I'll drop into the repo working dir)

| Source file (current name) | Rename to | Purpose |
|---|---|---|
| `deutsch-fuer-daenen.html` | `a2-b2-trainer.html` | Vocab/grammar/false-friends/sentence-builder quiz app |
| `ein-tag-in-berlin.html` | `tag-in-berlin.html` | "A day in Berlin" story-mode lesson |
| `index.html` | _(keep)_ | Landing hub — already links to the two slugs above |

> The landing page links to `./a2-b2-trainer.html` and `./tag-in-berlin.html`, so the renames above are required. If you keep the original names instead, update the two `href`s in `index.html` to match.

## Target structure

```
.
├── index.html            # landing hub (provided)
├── a2-b2-trainer.html     # exercise 1
├── tag-in-berlin.html     # exercise 2
├── .nojekyll              # tell GitHub Pages to skip Jekyll processing
└── README.md              # short project readme
```

## Tasks

1. **Init repo** in the current directory. Move/rename the three HTML files into place per the table above.
2. **Add `.nojekyll`** (empty file) so Pages serves the files verbatim.
3. **Write `README.md`** — one short paragraph (what this is: two interactive German exercises for Danish learners), how to run locally (`python3 -m http.server` then open `localhost:8000`), and the live URL placeholder.
4. **Verify locally:** start a static server, confirm all three pages load and that the landing-page links resolve (no 404s). The exercises have no backend, so a 200 on each file + visible content is enough.
5. **Create the GitHub repo and push.** Use the `gh` CLI:
   ```bash
   git init && git add -A && git commit -m "Static German-learning exercises"
   gh repo create deutsch-fuer-daenen --public --source=. --remote=origin --push
   ```
   (Pick any repo name; `deutsch-fuer-daenen` is a suggestion.)
6. **Enable GitHub Pages** from the `main` branch, root folder:
   ```bash
   gh api -X POST "repos/{owner}/{repo}/pages" \
     -f "source[branch]=main" -f "source[path]=/" 2>/dev/null || \
   echo "If that 404s, enable manually: repo Settings → Pages → Source: Deploy from a branch → main / (root)."
   ```
7. **Return the live URL** (`https://<user>.github.io/<repo>/`) and paste the README into the final message. Note that the first Pages deploy can take 1–2 minutes.

## Acceptance criteria

- Repo is public and pushed to GitHub.
- All three pages load over HTTP locally with no console errors and no broken links.
- GitHub Pages is enabled on `main` / root; the live `index.html` URL works and both exercise links open.
- `.nojekyll` and `README.md` are present.

## Notes / constraints

- **Do not add a build system, package.json, or framework.** These are intentionally dependency-free static files; keep it that way.
- Fonts load from `fonts.googleapis.com` at runtime — fine for Pages. If you ever want fully offline/self-hosted assets, that's a separate optional task; don't do it now.
- Filenames use hyphens only (no leading underscores), so Jekyll wouldn't choke even without `.nojekyll` — but include it anyway as a safeguard.

## Optional polish (only if quick)

- Add a `favicon.svg` (a simple 🇩🇰/🇩🇪 or "DE" glyph) and reference it from all three pages.
- Add `<meta property="og:title">` / `og:description` to `index.html` for nicer link previews.
- If I later point a custom domain at it: add a `CNAME` file containing the domain and configure DNS.

## Alternative hosts (if not GitHub Pages)

Same files, zero config:
- **Cloudflare Pages / Netlify:** connect the repo, leave build command empty, set the publish/output dir to the repo root.
- **Quick test:** `npx serve .` or `python3 -m http.server`.
