# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this directory is

This is a personal career portfolio for senior leadership: a single-page static site built from an accomplishments tracker. It has no build, lint, or test step.

**Public repo:** `github.com/mvu-panw/mvu_panw_portfolio` (branch `main`). Every push to `main` deploys to GitHub Pages through `.github/workflows/pages.yml`, which copies only `index.html` and `assets/` into `_site/`. Pages must have **Settings → Pages → Source** set to **GitHub Actions** (a one-time manual step).

- `Mike PANW Accomplishments - Sheet1.pdf` is a 3-page export of a Google Sheet titled "Professional Accomplishments Tracker". The Google Sheet is the source of truth. The PDF is a snapshot, so ask for a fresh export rather than treating the PDF as current.
- `index.html` + `assets/style.css` are the site. Preview with `python3 -m http.server 8080`, or open the file directly (no `fetch()`, so `file://` works).

Read the PDF with poppler (`pypdf` is not installed). Its hyperlinks don't show up in the text output, so extract them separately:
```bash
pdftotext -layout "Mike PANW Accomplishments - Sheet1.pdf" -
python3 -c "import re;print(set(re.findall(rb'/URI\s*\((.*?)\)',open('Mike PANW Accomplishments - Sheet1.pdf','rb').read())))"
```
Only the webinar and AI 101 cells have embedded URIs. The site's other links (palo-sandbox-challenges.com, paloquantumlearn.com, and the two GitHub repos) were mapped from the sibling projects' git remotes.

## Site

- **The UI is copied from `~/nca-aiio`**: same header, numbered nav tabs, hero, `.cards` with `.weight` bars and footer. NVIDIA green is swapped for PANW orange (`--accent: #fa582d`). Colors are custom properties on `:root`, with a `prefers-color-scheme: dark` override. `assets/pan-mark.png` is copied from `~/ai_sec_by_design/assets/images/PAN-Color.png`.
- The hero shows the headshot `assets/mike_vu.png` (192×192, displayed as a 120px circle).
- Sections: headline stat tiles, "Impact by type" cards, and milestones split into "In progress" / "Delivered" groups (newest first). Each `article.milestone` has a `data-type` (`collaborative|sales|strategic|enablement`). The inline script filters on it from both the impact cards and the chips, hides empty groups, and updates `#ms-count`.
- **Counts are hardcoded.** When a milestone is added or changes type or status, update the stat tiles (total, delivered/in-progress split, customer-facing assets, partner teams), the impact card counts and `weight-bar` widths (scaled so the largest type is 100%), and the hero lede if needed.
- Titles and impact text are edited lightly from the sheet for a leadership audience (typos fixed, dates as "Jul 31, 2026"). Don't add claims that aren't in the tracker. The tracker's Technical type has 0 entries, so it has no card.
- The footer has the export date and the date links were last checked. Update both when you refresh from a new export.

## Tracker structure

Columns: Date, Accomplishment Title, Type, Impact & Key Results, Links and Resources, Metric / Business Value, Stakeholders & Partners, Status.

- **Date** is either `YYYY-MM-DD` or `Ongoing`.
- **Status** is `Completed` or `Ongoing`.
- **Type** is one of Sales, Strategic, Technical, Collaborative, or Internal Enablement. The header summary counts only the first four, plus a total. Internal Enablement rows count toward the total but have no tile of their own, so the tiles don't add up to the total.
- Pages 2 and 3 of the export repeat the header with no data rows.

## Where the entries live in `~`

Many entries correspond to sibling project directories (see `~/.claude/CLAUDE.md`). Use those projects when you need to back up or expand an entry:

| Tracker entry | Project |
|---|---|
| Sandbox Challenge (palo-sandbox-challenges.com) | `public_ctf_page ` (trailing space), internal twin `ctf_page` |
| Quantum Lunch and Learn (paloquantumlearn.com) | `quantum_lunch_learn_public` / `quantum_lunch_learn_2026` |
| Secure AI by Design webinar series | `ai_sec_by_design` |
| AI Infrastructure 101 district all-hands | `ai_101_preso` |
| Webinars (e.g. PHP2Shell) | `webinar_recordings` |
| Neocloud SME work | `lambda`, `nebius`, `Coreweave`, `neo-cloud-research-hub` |

## Sensitivity

The site is public and shows quota attainment and colleagues' names; the user chose to publish it. `.gitignore` keeps the tracker PDF (`*.pdf`) out of the repo. Don't commit it, and don't put anything on the site that isn't in the tracker.
