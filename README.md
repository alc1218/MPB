# MPB Benchmark — Scenario 2 Results

An interactive reference table of all algorithms benchmarked on the **Moving Peaks Benchmark (MPB) Scenario 2**, with support for filtering, sorting, ranking, statistical significance testing, visualisation, and exporting results.

🔗 **Live site:** <a href="https://alc1218.github.io/MPB/" target="_blank">https://alc1218.github.io/MPB/</a>

---

## Overview

The Moving Peaks Benchmark is a widely used dynamic optimisation benchmark introduced by Branke (1999). This repository collects, to our knowledge, **all published algorithms** evaluated on Scenario 2, and presents them in a searchable, filterable table.

Results are reported as **offline error (mean ± standard error)** across multiple independent runs.

If an entry is missing or incorrect, please contact us at [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).

---

## Page Layout

The page is organised from top to bottom as:

1. **Results table** — with stats bar and export buttons
2. **Filter panels** — benchmark filters (left) and display & algorithm filters (right)
3. **Trend Over Years chart**

---

## Features

### Two-column Filter Panel

**Benchmark Filters (left)**
- **Severity** (0–6), **Peaks** (1–200), **Dimensions** (10–100), **Frequencies** (500–10000) — toggle individual result columns on/off; each group has *all / none* shortcuts and an independent **Reset** button

**Display & Algorithm Filters (right)**
- **Algorithm Category** — filter by algorithmic subfamily (PSO, DE, GA, Firefly, etc.); grouped under full family names (*Swarm Intelligence*, *Differential Evolution*, *Evolutionary Algorithm*, *Direct Search*); hovering any chip instantly shows the full subcategory name; chips automatically disable when no algorithms of that subcategory are present in the current view; user-deselected chips are not force-reselected by other filters
- **Hybrid** — when toggled off, algorithms combining more than one family are excluded
- **Year Range** — restrict results to a specific publication period; values are clamped to the data range on leaving the field
- **Top K by Average Rank** — show only the K best algorithms by average rank; K is always clamped to the number of available algorithms (enforced on every keystroke and on blur); the average rank column is recomputed within the Top K subset
- **Statistical Significance** — enable/disable Welch's two-tailed *t*-test against the best result per column; results not significantly worse than the best (p ≥ threshold) are shown in **bold\***; the p-value threshold is configurable (default 0.05, range 0.001–0.999, spinner step 0.001)
- **Best-in-class** — keep only algorithms that hold the lowest value in at least one visible column
- **Reset** (right panel) — resets all display & algorithm filters independently

### Table

- **Average Rank** — average rank across visible result columns; ties averaged; missing data not penalised; recomputed within the Top K subset when active
- **Algorithm Category** — colour-coded subcategory badge (teal = SI, rose = DE, orange = EA, indigo = DS); hovering shows the full name; hybrids show multiple badges
- **Best value** — shown in **bold** with a colour-coded background
- **Non-significant values** — shown in **bold\*** when Welch's test gives p ≥ threshold vs. the best
- **Sort** — click any column header; missing values always sort to the end
- **Linked paper titles** — link to source PDF or publisher page

### Statistical Significance Test

Results are tested using **Welch's two-sample *t*-test** (two-tailed):

- **Reference:** Welch, B.L. (1947). *The generalization of 'Student's' problem when several different population variances are involved.* Biometrika, 34(1/2), 28–35.
- Each algorithm uses its own individual run count *n* for the Welch–Satterthwaite degrees-of-freedom calculation
- The *p*-value is computed exactly via the regularised incomplete beta function
- The p-value threshold is adjustable and reflected dynamically in the table and all exports

### Trend Over Years

- **Column selector** — any result column; defaults to Severity 1
- **Metric** — best result per year or median result per year
- Plots the full dataset regardless of active table filters

### Export

All LaTeX files are bundled into a single **ZIP file**.

- **CSV** — visible rows and columns; if significance is active, adds a `_sig` column per result (`ns` = not significantly different from best, `sig` = significantly worse)
- **`main.tex`** — standalone LaTeX document with all packages (`booktabs`, `multirow`, `graphicx`, `amsmath`, `amssymb`, `hyperref`, `natbib`), `\input{}` for each table, bibliography, and overview text
- **Result tables** — up to 4 `.tex` files (one per condition: Severity, Peaks, Dimensions, Frequencies); algorithms sorted by Average Rank; columns: Method (with `\cite{}`), Year, Runs, results; best values in `\textbf{}`; non-significant values in `\textbf{..}$^{*}$`
- **`MPB_Scenario2_statistical_test.tex`** — generated when significance is active; contains three sections: *The Moving Peaks Benchmark* (description, Scenario 2 fitness function), *Performance Metrics* (Current Error, Offline Error with known properties, Standard Error of the Mean), and *Statistical Significance Testing* (full mathematical derivation)
- **`MPB_Scenario2_references.bib`** — BibTeX entries for all exported algorithms; always includes Branke (1999), Welch (1947), and Wilcoxon (1945) when significance is active

Compile with: `pdflatex main.tex && bibtex main && pdflatex main.tex && pdflatex main.tex`

---

## Algorithm Categories

| Family | Colour | Subcategories |
|---|---|---|
| **Swarm Intelligence** | Teal | PSO, Firefly, Fireworks, Cuckoo Search, Fish Swarm, ALO, WDO |
| **Differential Evolution** | Rose | DE |
| **Evolutionary Algorithm** | Orange | GA, AIS, EDA, CMA-ES |
| **Direct Search** | Indigo | Direct Search, Local Search, Extremal Optimization |

Hybrid algorithms are assigned to multiple subcategories (e.g. CDEPSO → DE + PSO).

---

## Repository Structure

```
.
├── index.html   # Fully self-contained webpage (layout, styles, data, logic)
└── README.md
```

> **Note:** The page is entirely self-contained in a single file. The benchmark data is stored as plain text directly inside `index.html` — no separate data file, no build step, no dependencies.

---

## Deployment (GitHub Pages)

1. Fork or clone this repository
2. Go to **Settings → Pages**
3. Under *Source*, select `main` branch and `/ (root)` folder
4. Click **Save** — your site will be live at `https://<your-username>.github.io/<your-repo>/`

---

## Adding or Updating Results

The data lives directly inside `index.html` as a plain semicolon-delimited string assigned to `const RAW_CSV`. To add or correct an entry:

1. Open `index.html` in any text editor
2. Find the `const RAW_CSV = \`` block near the top of the `<script>` section
3. Add a new row or edit an existing one following the same semicolon-separated format
4. Save, commit, and push

The `Category` field uses `subcategory|MainFamily`, with multiple categories for hybrids separated by commas (e.g. `DE|DE,PSO|SI`). Empty result fields are left blank between consecutive semicolons.

---

## Column Reference

| Group | Columns | Fixed parameters |
|---|---|---|
| **Severity** | Sev. 0 – Sev. 6 | Peaks = 10, Dim = 5, Freq = 5000 |
| **Peaks** | P = 1, 5, 20, 30, 40, 50, 100, 200 | Sev. = 1, Dim = 5, Freq = 5000 |
| **Dimensions** | D = 10, 20, 50, 100 | Sev. = 1, Peaks = 10, Freq = 5000 |
| **Frequencies** | f = 500, 1000, 2000, 2500, 3000, 10000 | Sev. = 1, Peaks = 10, Dim = 5 |

Each result cell displays `mean ± std. error`. A dash (`—`) indicates the condition was not reported in the original paper.

---

## Key References

- **MPB:** Branke, J. (1999). *Memory enhanced evolutionary algorithms for changing optimization problems.* IEEE CEC 1999, pp. 1875–1882.
- **Welch's t-test:** Welch, B.L. (1947). *The generalization of 'Student's' problem when several different population variances are involved.* Biometrika, 34(1/2), 28–35.
- **Wilcoxon test:** Wilcoxon, F. (1945). *Individual comparisons by ranking methods.* Biometrics Bulletin, 1(6), 80–83.

---

## Author

Created by **Arcadi Llanza**.

---

## Citation

If you use this collection in your research, please cite the individual papers listed in the table. To cite the benchmark table itself, contact [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).

---

## Contact

Missing an entry? Found an error? Reach out at [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).
