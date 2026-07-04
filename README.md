# Signal — AI &amp; Vendor Risk Vetting

A self-service web app for pre-screening AI tools and third-party services before submitting them to District Information Security for approval. You describe a tool, answer plain-language questions, and Signal produces a **RED / YELLOW / GREEN** report aligned to the **EDUCAUSE HECVAT 4** framework — the standard hundreds of higher-ed institutions use to vet vendors.

Built for the North Orange County Community College District (NOCCCD) vendor-review workflow.

## What it does

- **Structured intake** — name, vendor, purpose, and (critically) what institutional data the tool would touch.
- **Self-scoring rubric** — questions grouped into the HECVAT 4 domains (Company &amp; governance, Data protection, AI/ML practices, Privacy &amp; data lifecycle, Integration &amp; access scope, Operations). Each answer carries a weight; four questions are flagged **critical** and can cap the rating.
- **Live verdict** — a weighted score and provisional RED/YELLOW/GREEN updates as you answer.
- **CISO documentation checklist** — maps directly to the eight items a security officer typically requests (SOC 2 Type II, data flow diagram, AI data-handling policy, sub-processor list, encryption/access control, retention/deletion, OAuth scopes, no-training confirmation).
- **NOCCCD policy alignment** — every rubric question is cross-referenced to the actual NOCCCD Board Policy / Administrative Procedure it relates to (e.g. AP 3722 for data-sharing agreements, BP/AP 5040 for FERPA), with a link to the real policy PDF. Where no NOCCCD policy exists yet (there is no adopted AI-specific BP/AP as of this writing), the report says so explicitly instead of citing something that doesn't exist.
- **Exportable report** — print to PDF, copy as Markdown, export raw JSON, or export a **standalone shareable report page** (see below).
- **Library** — every assessment is saved in the browser so you build a running record of reviewed tools.
- **Plain-Talk glossary** — every IT term translated into plain English, to lower the barrier for non-technical faculty and staff.

## Saving and showing a report to others

Signal has no backend, so "saving and sharing" happens three ways, in increasing order of durability:

1. **Share link** (`🔗 Copy share link`) — encodes the whole assessment into a URL fragment. Fastest option, but the URL can get long for a heavily-annotated review, and some chat/email clients truncate very long links.
2. **Standalone report page** (`⤓ Export shareable page`) — downloads a single self-contained `.html` file with the rendered report baked in. This doesn't depend on the recipient's browser storage or on a fragile URL — open it, email it, or drop it anywhere.
3. **Publish to `/reports/`** — commit an exported standalone report into this repo's `reports/` folder and push. Once GitHub Pages redeploys, it's a permanent, stable link (e.g. `https://profgarygraves.github.io/signal-vetting/reports/superhuman.html`) you can hand to NOCCCD without them needing the app at all. `reports/index.html` is a small gallery listing everything published this way — add a card there when you publish a new one.

## How the verdict works

- A **weighted average** is computed across all answered categories (0–100).
- A **missing critical control** (independent audit, encryption, no-AI-training, or connection scope) caps the verdict at **RED**, regardless of overall score.
- An **unconfirmed critical control** caps the verdict at **YELLOW**.
- Otherwise: 80+ = GREEN, 55–79 = YELLOW, below 55 = RED.

This mirrors HECVAT's use of asterisked "critical" questions and its High-Risk Evaluation tab, where a single critical gap can be enough to reject a vendor.

## Running it

It's a single static file. No build step, no dependencies, no server.

**Locally:** open `index.html` in any browser.

**On GitHub Pages:**
1. Create a new repository (e.g. `signal-vetting`) under your account.
2. Upload `index.html` and `.nojekyll` to the repo root.
3. Go to **Settings → Pages**, set the source to the `main` branch / root folder.
4. Your site goes live at `https://profgarygraves.github.io/signal-vetting/`.

The `.nojekyll` file tells GitHub Pages to serve the files as-is.

## Data &amp; privacy

Everything runs in the browser. Assessments are stored in `localStorage` on the device you used — nothing is sent anywhere. Use **Export data** on any report to keep a portable JSON copy or move it between machines.

## Customizing the rubric

All questions, weights, critical flags, explanations, and glossary entries live in two arrays near the top of the `<script>` block in `index.html`: `RUBRIC` and `GLOSSARY`. They're plain JavaScript objects — edit the text, change a weight, add a question, or mark something critical without touching the rest of the app. As Andy's TPRM program and the HECVAT template evolve, this is the only place you need to update.

## Updating the NOCCCD policy citations

The `POLICIES` object and each question's `policy: [...]` array (or `policyGap` string, for items no adopted policy yet covers) live right above `RUBRIC` in `index.html`. Each entry was verified against the actual PDF text published at nocccd.edu, not assumed from generic community-college template numbering — if NOCCCD adopts a new AI-specific policy, add it to `POLICIES` and swap the `policyGap: AI_POLICY_GAP` on the AI-section questions for a real `policy: [...]` reference.

---

*Signal is a pre-screen that supports, not replaces, formal review by the District Information Security Office.*
