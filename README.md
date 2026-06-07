# Doy Kim — Academic CV

A reproducible academic CV built with [`{vitae}`](https://pkg.mitchelloharawild.com/vitae/) (awesomecv template), rendered to PDF via GitHub Actions, and served as a permanent link through GitHub Pages.

**Live PDF:** https://doy-kim-research.github.io/cv-and-resume/cv.pdf

---

## Repository Structure

```
cv/
├── cv.Rmd                         # Source document (edit sections here)
├── data/
│   ├── education.csv              # Degrees
│   ├── positions.csv              # Roles / affiliations (one row per position)
│   ├── position_details.csv       # Bullet points for each position
│   ├── awards.csv                 # Awards and fellowships
│   ├── skills.csv                 # Skills by category
│   ├── service.csv                # Leadership, peer review, presentations
│   ├── publications_peer.bib      # Peer-reviewed publications (BibTeX)
│   └── publications_other.bib     # Non-peer-reviewed publications (BibTeX)
├── docs/
│   └── cv.pdf                     # Auto-generated; do not edit manually
└── .github/workflows/
    └── render-cv.yml              # CI/CD pipeline
```

---

## One-time Setup

### 1. Prerequisites (local machine)

```r
install.packages(c("vitae", "tidyverse", "glue", "tinytex", "rmarkdown", "RefManageR"))
tinytex::install_tinytex()
tinytex::tlmgr_install(c(
  "fontawesome5", "tcolorbox", "pgf",
  "environ", "trimspaces", "etoolbox"
))
```

### 2. GitHub repository

1. Create a new repository named **`cv-and-resume`** under your account (`doy-kim-research`).
2. Push this entire folder to the `main` branch.
3. In **Settings → Pages**, set Source to **Deploy from a branch**, Branch = `main`, Folder = `/docs`.
4. Your permanent PDF URL will be: `https://doy-kim-research.github.io/cv-and-resume/cv.pdf`

### 3. GitHub Actions permissions

In **Settings → Actions → General**, set Workflow permissions to **Read and write permissions**.

---

## Updating the CV

### Adding a new entry (most common tasks)

| Task | File to edit |
|---|---|
| New degree | `data/education.csv` |
| New position / affiliation | `data/positions.csv` + `data/position_details.csv` |
| New award or fellowship | `data/awards.csv` |
| New publication | `data/publications_peer.bib` or `data/publications_other.bib` |
| New service item | `data/service.csv` |
| New skill | `data/skills.csv` |

### Workflow after any edit

```bash
git add .
git commit -m "add: PME-NA 2026 paper"
git push
```

GitHub Actions renders the Rmd automatically on push. Within ~5 minutes, the PDF at your permanent URL is updated. No manual sharing required.

### Rendering locally

```r
rmarkdown::render("cv.Rmd")
```

---

## Data Schemas

### `positions.csv`

| Column | Description |
|---|---|
| `id` | Integer key; must match `position_id` in `position_details.csv` |
| `role` | Job title as it should appear on the CV |
| `organization` | Full organization name |
| `location` | City, State/Country |
| `when` | Date string (e.g., `Sep 2024--Present`); pre-formatted to allow non-standard ranges like `"Aug 2021--May 2022; Aug 2024--Aug 2025"` |

### `position_details.csv`

| Column | Description |
|---|---|
| `position_id` | Foreign key matching `id` in `positions.csv` |
| `order` | Integer controlling display order within a position |
| `project` | Project name (rendered in italics as a label prefix); leave blank if no named project |
| `detail` | Bullet point text |

### `service.csv`

| Column | Description |
|---|---|
| `category` | Controls which subsection the entry appears in: `Research Mentorship`, `Peer Review`, `Committee Service`, `Presentations`, `Panel Discussions` |
| `role` | Your role or talk title |
| `organization` | Venue, event, or organization |
| `when` | Date or date range string |
| `description` | Semicolon-delimited bullet points (leave blank for `Peer Review` entries) |

---

## Customization

### Accent color

Open `cv.Rmd` and add to the YAML header:

```yaml
output:
  vitae::awesomecv:
    color: "0D47A1"   # any hex color without the #
```

### Author name bolding in publications

`bibliography_entries()` does not bold a specific author name by default. To bold your name, add a custom CSL file and reference it in the YAML:

```yaml
csl: apa-author-bold.csl   # place this file in the repo root
```

A template CSL for author bolding is available at: https://github.com/citation-style-language/styles

---

## Known Limitations

- PDF only (no web version); add a Quarto HTML render step if a web version is needed.
- First GitHub Actions run takes ~8--10 minutes due to TinyTeX installation; subsequent runs are faster due to package caching.
- Author name bolding in publications requires a custom CSL file (see Customization above).
