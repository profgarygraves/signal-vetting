# Signal — Setup &amp; Deployment Brief

A handoff note for picking this project up in Claude Code (or any terminal) and getting it live on GitHub Pages.

## What this project is

`Signal` is a single-file static web app (`index.html`) — a self-service vetting tool that scores AI tools and third-party services RED/YELLOW/GREEN against the EDUCAUSE HECVAT 4 framework, for submission to District Information Security at NOCCCD. No build step, no dependencies, no server. See `README.md` for the full feature description.

## Files in this folder

- `index.html` — the entire app (HTML + CSS + JS in one file)
- `README.md` — project overview, rubric logic, customization notes
- `SETUP.md` — this file
- `.nojekyll` — empty file that tells GitHub Pages to skip Jekyll processing (optional; the site works without it)

## Target repo

Empty repo already created at: `https://github.com/profgarygraves/signal-vetting`

## First-time push (run these in the project folder)

```bash
git init
git add .
git commit -m "Initial commit: Signal AI vetting tool"
git branch -M main
git remote add origin https://github.com/profgarygraves/signal-vetting.git
git push -u origin main
```

The push will prompt for GitHub credentials the first time. Use a Personal Access Token as the password (GitHub no longer accepts account passwords for git over HTTPS). Generate one at: GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → scope `repo`.

## Turn on GitHub Pages

1. Repo → **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: **main**, folder: **/ (root)** → **Save**
4. Live in ~1 minute at: `https://profgarygraves.github.io/signal-vetting/`

## Making future edits

Edit `index.html`, then:

```bash
git add index.html
git commit -m "Describe the change"
git push
```

Pages redeploys automatically within a minute or two.

## Where to change the rubric

All questions, weights, critical flags, plain-language explanations, and the glossary live in two JavaScript arrays near the top of the `<script>` block in `index.html`: `RUBRIC` and `GLOSSARY`. Edit those to adjust scoring or wording as the NOCCCD TPRM / HECVAT requirements evolve. Nothing else needs to change.

## Ideas parked for later

- A "meeting-recorder" question branch for tools like Fireflies.ai / Otter.ai (recording consent, transcript retention, who's in the room)
- An "email this report to Andy" button on the report view
- A printable cover sheet that pre-fills the requester / department fields
