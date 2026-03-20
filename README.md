# MPB Benchmark — Scenario 2 Results

An interactive reference table of all algorithms benchmarked on the **Moving Peaks Benchmark (MPB) Scenario 2**, with support for filtering, sorting, ranking, statistical significance testing, visualisation, and exporting results.

🔗 **Live site:** <a href="https://alc1218.github.io/MPB/" target="_blank">https://alc1218.github.io/MPB/</a>

---

## Overview

The Moving Peaks Benchmark is a widely used dynamic optimisation benchmark. This repository collects, to our knowledge, **all published algorithms** evaluated on Scenario 2, and presents them in a searchable, filterable table.

Results are reported as **offline error (mean ± standard error)** across multiple independent runs.

If an entry is missing or incorrect, please contact us at [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).

---

## Page Layout

The page is organised in the following order from top to bottom:

1. **Results table** — with stats bar and export buttons
2. **Filter panels** — benchmark filters (left) and display & algorithm filters (right)
3. **Trend Over Years chart**

---

## Features

### Two-column Filter Panel

**Benchmark Filters (left)**
- **Severity** (0–6), **Peaks** (1–200), **Dimensions** (10–100), **Frequencies** (500–10000) — toggle individual result columns on/off using chip-style checkboxes; each group has *all / none* shortcuts and an individual **Reset** button

**Display & Algorithm Filters (right)**
- **Algorithm Category** — filter by algorithmic subfamily (e.g. PSO, DE, GA, Firefly); subcategories are grouped under their full family name (*Swarm Intelligence*, *Differential Evolution*, *Evolutionary Algorithm*, *Direct Search*); hovering any chip instantly shows the full subcategory name; chips are automatically enabled or disabled in sync with the table; user-deselected chips are not force-reselected by other filters
- **Hybrid** — toggle to hide algorithms combining more than one algorithmic family (e.g. DE + PSO, Direct Search + CMA-ES)
- **Year Range** — restrict results to a specific publication period; values are clamped to the data range on leaving the field
- **Top K by Average Rank** — display only the K best-performing algorithms; K is always clamped to the number of available algorithms (enforced on every keystroke and on blur); the average rank is recomputed within the Top K subset
- **Statistical Significance** — toggle Welch's two-tailed *t*-test against the best algorithm per column; results not significantly worse than the best (at the chosen p-value threshold) are marked **bold with a superscript \***; the p-value threshold (default 0.05) can be adjusted dynamically using a spinner with step 0.001 in the range (0.001, 0.999)
- **Best-in-class** — keep only algorithms that hold the lowest value in at least one currently visible column
- **Reset** — resets only the Display & Algorithm Filters

### Table

- **Average Rank column** — average rank across all visible result columns (lower = better); ties resolved by averaging; recomputed within the visible subset when Top K is active
- **Algorithm Category column** — colour-coded subcategory badge; hovering shows the full name; hybrid algorithms show multiple badges; teal = Swarm Intelligence, rose = Differential Evolution, orange = Evolutionary Algorithm, indigo = Direct Search
- **Best value highlighting** — the lowest value per column is shown in **bold**; values not significantly worse are shown in **bold\*** when significance testing is active
- **Sort** — click any column header to sort ascending or descending; missing values (—) always sort to the end
- **Linked paper titles** — paper titles link directly to the source PDF or publisher page

### Statistical Significance Testing

Statistical significance is assessed using **Welch's two-sample *t*-test** (two-tailed), as introduced in:

> Welch, B.L. (1947). *The generalization of 'Student's' problem when several different population variances are involved.* **Biometrika**, 34(1/2), 28–35.

Since only the mean, standard error, and number of runs are stored (not raw per-run data), Welch's *t*-test is used as a statistically valid approximation. Each algorithm uses its own individual run count for the degrees-of-freedom calculation. The *p*-value is computed exactly via the regularised incomplete beta function.

A result is marked **\*** if p ≥ threshold (cannot reject that it is equal to the best). The threshold is adjustable and defaults to 0.05.

### Trend Over Years

A line chart showing how benchmark performance has evolved across publication years:

- **Column selector** — defaults to Severity 1 (standard benchmark condition)
- **Metric selector** — best result per year or median result per year
- Plots the full dataset regardless of active table filters

### Export

- **Export to CSV** — currently visible rows and columns; includes a `_sig` column per result column (`ns` = not significant, `sig` = significant) when significance testing is active; a comment line explains the coding
- **Export to LaTeX** — up to 4 `.tex` files (one per experimental condition); best values wrapped in `\textbf{}`; non-significant values wrapped in `\textbf{}$^{*}$`; table captions document the notation and cite the Welch reference; algorithm names include `\cite{}` references; `booktabs` formatting
- **Export references** — `MPB_Scenario2_references.bib` with BibTeX entries for all cited algorithms, plus Welch (1947) and Wilcoxon (1945) when significance testing is active
- **Statistical test description** — `MPB_Scenario2_statistical_test.tex` is generated when significance is active; contains the full mathematical formulation of the test (t-statistic, Welch–Satterthwaite df, exact p-value via regularised incomplete beta), interpretation of bold/bold\*/plain notation, and the current p-threshold value

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

The data lives inside `index.html` as a semicolon-delimited string assigned to `const RAW_CSV`. To add or correct an entry:

1. Open `index.html` in any text editor
2. Find the `const RAW_CSV = \`` block near the top of the `<script>` section
3. Add a new row or edit an existing one following the same format
4. Save, commit, and push

The `Category` field uses `subcategory|MainFamily`, with multiple categories separated by commas (e.g. `DE|DE,PSO|SI`). Empty result fields are left blank between consecutive semicolons.

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

## Author

Created by **Arcadi Llanza**.

---

## Citation

If you use this collection in your research, please cite the individual papers listed in the table. To cite the benchmark table itself, contact [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).

---

## Contact

Missing an entry? Found an error? Reach out at [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).
