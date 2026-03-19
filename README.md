# MPB Benchmark — Scenario 2 Results

An interactive reference table of all algorithms benchmarked on the **Moving Peaks Benchmark (MPB) Scenario 2**, with support for filtering, sorting, ranking, visualisation, and exporting results.

🔗 **Live site:** <a href="https://alc1218.github.io/MPB/" target="_blank">https://alc1218.github.io/MPB/</a>

---

## Overview

The Moving Peaks Benchmark is a widely used dynamic optimisation benchmark. This repository collects, to our knowledge, **all published algorithms** evaluated on Scenario 2, and presents them in a searchable, filterable table.

Results are reported as **offline error (mean ± standard error)** across multiple independent runs.

If an entry is missing or incorrect, please contact us at [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).

---

## Features

### Filtering

- **Toggle columns by group** — independently show or hide individual columns within Severity (0–6), Peaks (1–200), Dimensions (10–100), and Frequencies (500–10000) using chip-style checkboxes; use the *all / none* shortcuts per group
- **Filter by year range** — restrict results to a specific publication period using the min/max year inputs
- **Algorithm Category filter** — filter algorithms by algorithmic subfamily (e.g. PSO, DE, GA, AIS, Firefly, Direct Search); subcategories are grouped by main family (SI, DE, EA, DS) with *all / none* shortcuts; chips are automatically enabled or disabled in sync with the table — if no algorithm of a given subcategory is present in the current view, the chip is faded and deselected; hybrid algorithms belong to multiple subcategories and remain visible when any of their subcategories is active; user-deselected chips are not automatically re-selected when filters change
- **Top K by Average Rank** — enter a value K and click *Show Top K* to restrict the table to the K best-performing algorithms by average rank; the filter updates live as K is changed; works on top of all other filters
- **Best-in-class filter** — toggle *"Show only best-in-class algorithms"* to keep only algorithms that hold the lowest value in at least one currently visible column; the count of qualifying algorithms is shown inline

### Trend Over Years

A line chart displayed above the table shows how benchmark performance has evolved across publication years:

- **Column selector** — choose any result column from all four experimental groups; defaults to Severity 1 (the standard benchmark condition)
- **Metric selector** — toggle between *best result per year* (minimum offline error published that year) and *median result per year*
- The chart always plots the full dataset, independent of the active table filters, to give a complete historical view
- Updates automatically when the selected column or metric changes

### Table

- **Average Rank column** — the first column shows each algorithm's average rank across all visible result columns (lower = better, rank 1); ties are resolved by averaging; algorithms with no data in a column are not penalised; the table is sorted by this column by default
- **Algorithm Category column** — displays a colour-coded subcategory badge (e.g. `PSO`, `DE`, `Firefly`) for each algorithm; hybrid algorithms show multiple badges; colour coding: blue = SI, pink = DE, green = EA, yellow = DS
- **Best value highlighting** — the lowest value in each result column is shown in **bold** with a subtle colour-coded background (purple = Severity, blue = Peaks, green = Dimensions, amber = Frequencies)
- **Sort** — click any column header to sort ascending or descending; clicking again reverses the direction
- **Linked paper titles** — paper titles link directly to the source PDF or publisher page where available

### Export

- **Export to CSV** — downloads the currently visible rows and columns as a `.csv` file, including the full algorithm category description (e.g. `SI (PSO)`, `DE (DE) + SI (PSO)` for hybrids)
- **Export to LaTeX** — downloads up to 4 separate `.tex` files, one per experimental condition (Severity, Peaks, Dimensions, Frequencies); a file is only generated if at least one visible column has data for the filtered algorithms; best values per column are wrapped in `\textbf{}`; algorithm names include `\cite{}` references; files use `booktabs` formatting and are ready to `\input{}` directly into a paper
- **Export references** — a `MPB_Scenario2_references.bib` file is downloaded alongside the LaTeX tables, containing only the BibTeX entries for the algorithms in the current export

---

## Algorithm Categories

Algorithms are classified into the following subcategories, grouped by main family:

| Family | Subcategories |
|---|---|
| **SI** — Swarm Intelligence | PSO, Firefly, Fireworks, Cuckoo Search, Fish Swarm, ALO, WDO |
| **DE** — Differential Evolution | DE |
| **EA** — Evolutionary Algorithms | GA, AIS, EDA, CMA-ES |
| **DS** — Direct Search | Direct Search, Local Search, Extremal Optimization |

Hybrid algorithms are assigned to multiple subcategories (e.g. CDEPSO → DE + PSO, EFDS (FDS+CMA-ES) → Direct Search + CMA-ES).

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

No build step or external dependencies are required. The page is fully static.

---

## Adding or Updating Results

The data lives directly inside `index.html` as a plain semicolon-delimited string assigned to `const RAW_CSV`. To add or correct an entry:

1. Open `index.html` in any text editor
2. Find the `const RAW_CSV = \`` block near the top of the `<script>` section
3. Add a new row or edit an existing one following the same semicolon-separated format as the other rows
4. Save, commit, and push

The column order matches the header row at the top of the `RAW_CSV` block. The `Category` field uses the format `subcategory|MainFamily`, with multiple categories for hybrid algorithms separated by commas (e.g. `DE|DE,PSO|SI`). Empty result fields are left blank between two consecutive semicolons.

---

## Column Reference

Results are grouped into four experimental conditions:

| Group | Columns | Fixed parameters |
|---|---|---|
| **Severity** | Sev. 0 – Sev. 6 | Peaks = 10, Dim = 5, Freq = 5000 |
| **Peaks** | P = 1, 5, 20, 30, 40, 50, 100, 200 | Sev. = 1, Dim = 5, Freq = 5000 |
| **Dimensions** | D = 10, 20, 50, 100 | Sev. = 1, Peaks = 10, Freq = 5000 |
| **Frequencies** | f = 500, 1000, 2000, 2500, 3000, 10000 | Sev. = 1, Peaks = 10, Dim = 5 |

Each result cell displays `mean ± std. error`. A dash (`—`) indicates the condition was not reported in the original paper.

---

## Citation

If you use this collection in your research, please cite the individual papers listed in the table. If you wish to cite the benchmark table itself, please contact us at [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).

---

## Contact

Missing an entry? Found an error? Reach out at [mpb.benchmark@gmail.com](mailto:mpb.benchmark@gmail.com).
