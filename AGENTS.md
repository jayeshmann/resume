# Resume project: agent guide

Single-file LaTeX resume for Jayesh Mann. Compiles to a single PDF via pdflatex (CI in `.github/workflows/` or Overleaf).

## Source of truth

- **`Havard Resume Guide.pdf`** (Harvard Office of Career Services, 2020) is the authoritative style spec for this resume. Re-read it before changing format, font, margins, section order, section names, or adding any visual element not already present (icons, graphics, color, multi-column, photos).
- **`resume.tex`** is the only file that produces the PDF. No `\input` or `\include`, no auxiliary `.tex` files.
- The deployed PDF lives at `Jayesh_Mann_Resume.pdf` in repo root (auto-committed by CI on every push to `main`). The filename is self-identifying so recruiters who download via the TinyURL get a sensibly-named file in their downloads folder.

## Formatting rules (must hold)

- **Font:** Times New Roman via `newtxtext` + `newtxmath`. Body 11 pt.
- **Margins:** 1 inch all sides.
- **Section headers:** bold, uppercase, horizontal rule below (titlesec config in `resume.tex`).
- **Bullets:** plain `\textbullet`. No icons. Do not re-introduce the `fontawesome` package.
- **Page count:** 1 or 2 pages, both acceptable per the guide. Never go above 2.
- **Reverse chronological** within each section (most recent first).
- **Section order** (by importance for a 5+ YOE engineer): Summary → Experience → Projects and Open Source → Achievements and Awards → Skills → Education.

## Content rules

- **No `—` (em-dashes) or `–` (en-dashes) in prose.** En-dashes from `--` inside LaTeX date ranges are fine (they render as typographic en-dashes, not prose punctuation).
- **No personal pronouns.** Never use "I", "my", "we", "our" in the resume body.
- **No photos, age, sex, references list.**
- **Action verb first** in every bullet. Quantify and qualify (numbers, scale, percentage, time).
- **Don't start lines with dates.** Dates go right-aligned via `\hfill`.
- **Don't abbreviate** credentials (e.g., spell out "Bachelor of Computer Applications"). Industry-standard tech abbreviations are OK (DAU, K8s, OCR, OSS, ODBC/JDBC).
- **Hidden ATS keyword layer** (1 pt white text at top of document) is intentional; keep it factual, no prompt injection. PDF metadata mirrors it.

## Load-bearing facts (do not regress)

- Current team at Gojek/Gopay is **Wallet team** (not "Discovery").
- Bills platform: contributed to **re-architecture and new features**, not a from-scratch build.
- Order Management: built the **async post-processing pipeline only**, not sync order processing or the system architecture.
- Split Bill: **17K splits/day is the create API only**; full system has 15+ APIs (auto/manual settlement, 4 draft flows, create/get/update/discard) with much higher total traffic.
- GoComet role: started **Mar 2026**, Senior AI Engineer and Architect.
- Nova: **owned end to end** by Jayesh. Greenfield agentic AI platform.
- DataHub on Nova: used for **context provision**, not retrieval.
- Nova target: **50 major enterprise customers** (Unilever, Honda, Tata Motors, Schneider Electric named).

## Build

- **CI:** `.github/workflows/build-resume.yml` runs on every push to `main` and on `workflow_dispatch`. Uses `actions/checkout@v5` (Node 24; v4 deprecates June 2026) and `xu-cheng/latex-action@v4` (most-starred LaTeX action, actively maintained). Sets `SOURCE_DATE_EPOCH=$(git log -1 --pretty=%ct)` so identical source produces byte-identical PDF (otherwise LaTeX bakes timestamps into `/CreationDate` and every build looks like a content change). Commits `Jayesh_Mann_Resume.pdf` back to `main` with `[skip ci]` in the message to belt-and-suspender against future PAT-based pushes. Force-moves a `YYYYMMDD` date tag to the new HEAD so each day has one mutable version pointer (last push of the day wins, intentional). Uses default `GITHUB_TOKEN` with `permissions: contents: write` (no PAT, no secret). Git author for generated commits is `github-actions[bot]`.
- **Local:** `pdflatex resume.tex` on any TeXLive 2022+ install. Required packages: `newtxtext`, `newtxmath`, `geometry`, `hyperref`, `enumitem`, `titlesec`, `xcolor`, `microtype`.

## Do not

- Do not re-introduce `fontawesome` or any header icons.
- Do not restore: Coursework section, 2019 Trendsetters internship, 2014 chess tournament award. These were cut for relevance at 5+ YOE, not for space.
- Do not add a "Stack:" line per role; the Skills section covers tech.
- Do not pad spacing or add sections beyond what fits in 2 pages.
- Do not speculatively add sections, skills, or experience the user hasn't supplied.
- Do not change the keyword metadata block without keeping it factual.

## Branching and commit policy

- Never use PRs or branches. All work goes to `main` directly.
- CI handles tagging; never tag manually.
- Codex pre-commit review (per user global CLAUDE.md) is optional for resume content edits; they fall under the "non-runbook doc" skip category. Still mandatory for changes to `.github/workflows/`, `resume.tex` build config, or any `.sh`/script files.
