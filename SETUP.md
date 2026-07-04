# Signal — Setup &amp; Deployment Brief

A handoff note for picking this project up in Claude Code (or any terminal) and getting it live on GitHub Pages.

## What this project is

`Signal` is a single-file static web app (`index.html`) — a self-service vetting tool that scores AI tools and third-party services RED/YELLOW/GREEN against the EDUCAUSE HECVAT 4 framework, for submission to District Information Security at NOCCCD. No build step, no dependencies, no server. See `README.md` for the full feature description.

## Files in this folder

- `index.html` — the entire app (HTML + CSS + JS in one file)
- `README.md` — project overview, rubric logic, customization notes
- `SETUP.md` — this file
- `.nojekyll` — empty file that tells GitHub Pages to skip Jekyll processing (optional; the site works without it)
- `reports/` — published, standalone HTML snapshots of finalized reviews (see "Publishing a report" below), plus `reports/index.html`, a small gallery listing them

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

## Publishing a report to NOCCCD

1. Finish and finalize an assessment in the app, then click **⤓ Export shareable page** on the report view — it downloads `<tool-name>-signal-report.html`, a fully self-contained snapshot (no localStorage, no app dependency).
2. Move that file into `reports/` in this repo, e.g. `reports/superhuman.html`.
3. Add a card for it in `reports/index.html` (copy an existing `<a class="card">` block and update the name/vendor/verdict/score/date).
4. `git add reports/ && git commit -m "Publish <tool> report" && git push`
5. Once Pages redeploys, the report is live at `https://profgarygraves.github.io/signal-vetting/reports/<file>.html` — that's the durable link to hand to NOCCCD. It's a static snapshot: if you re-answer questions in the app later, re-export and replace the file to update it.

## Where to change the rubric

All questions, weights, critical flags, plain-language explanations, and the glossary live in two JavaScript arrays near the top of the `<script>` block in `index.html`: `RUBRIC` and `GLOSSARY`. Edit those to adjust scoring or wording as the NOCCCD TPRM / HECVAT requirements evolve. Nothing else needs to change.

## Where the NOCCCD policy citations come from

The `POLICIES` object (right above `RUBRIC`) holds every cited Board Policy / Administrative Procedure — number, title, URL, and a one-line summary — verified against the actual PDF text at nocccd.edu, not assumed from generic template numbering. Each rubric question then references policy ids via `policy: [...]`, or, where no adopted NOCCCD policy covers it yet (the AI section — there's no adopted AI-specific BP/AP as of this review), a `policyGap` string explaining that plainly instead of inventing a citation.

## Ideas parked for later

- A "meeting-recorder" question branch for tools like Fireflies.ai / Otter.ai (recording consent, transcript retention, who's in the room)
- An "email this report to Andy" button on the report view
- A printable cover sheet that pre-fills the requester / department fields
