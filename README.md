# MPB Benchmark — Scenario 2 Results

An interactive reference table of all algorithms benchmarked on the **Moving Peaks Benchmark (MPB) Scenario 2**, with support for filtering, sorting, ranking, and exporting results.

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
- **Best-in-class filter** — toggle *"Show only best-in-class algorithms"* to keep only algorithms that hold the lowest value in at least one currently visible column; the count of qualifying algorithms is shown inline

### Table
- **Average Rank column** — the first column shows each algorithm's average rank across all visible result columns (lower value = rank 1); ties are resolved by averaging (e.g. two tied algorithms both receive rank 1.5); algorithms with no data in a column are not penalised for it; the table is sorted by this column by default
- **Best value highlighting** — the lowest value in each result column is shown in **bold** with a subtle colour-coded background (purple = Severity, blue = Peaks, green = Dimensions, amber = Frequencies)
- **Sort** — click any column header to sort ascending or descending; clicking again reverses the direction
- **Linked paper titles** — paper titles link directly to the source PDF or publisher page where available

### Export
- **Export to CSV** — downloads the currently visible rows and columns as a `.csv` file
- **Export to LaTeX** — downloads up to 4 separate `.tex` files, one per category (Severity, Peaks, Dimensions, Frequencies); a file is only generated if at least one visible column in that category has data for the filtered algorithms; best values are automatically wrapped in `\textbf{}`; files use `booktabs` formatting and are ready to `\input{}` directly into a paper

---

## Repository Structure

```
.
├── index.html   # Fully self-contained webpage (layout, styles, data, logic)
└── README.md
```

> **Note:** The raw CSV data is not stored in plaintext. It is base64-encoded and embedded directly inside `index.html`, decoded at runtime. The file is fully self-contained — no second file or build step is needed.

---

## Deployment (GitHub Pages)

1. Fork or clone this repository
2. Go to **Settings → Pages**
3. Under *Source*, select `main` branch and `/ (root)` folder
4. Click **Save** — your site will be live at `https://<your-username>.github.io/<your-repo>/`

No build step or external dependencies are required. The page is fully static.

---

## Adding or Updating Results

To add a new algorithm or correct an existing entry, edit the source CSV then re-embed it into `index.html`. The base64 string inside the file (assigned to `window._D`) must be replaced with the newly encoded version:

```bash
# Generate the new base64 string
cat MPB_results.csv | base64 | tr -d '\n'
```

Copy the output and replace the value of `window._D='...'` near the top of the `<script>` block in `index.html`. Then commit and push.

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
