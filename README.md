# MPB Benchmark — Scenario 2 Results

An interactive reference table of all algorithms benchmarked on the **Moving Peaks Benchmark (MPB) Scenario 2**, with support for filtering, sorting, and exporting results.

🔗 **Live site:** `https://alc1218.github.io/MPB/`

---

## Overview

The Moving Peaks Benchmark is a widely used dynamic optimisation benchmark. This repository collects, to our knowledge, **all published algorithms** evaluated on Scenario 2, and presents them in a searchable, filterable table.

Results are reported as **offline error (mean ± standard error)** across multiple independent runs.

If an entry is missing or incorrect, please contact us at [mpb_benchmark@gmail.com](mailto:mpb_benchmark@gmail.com).

---

## Features

- **Filter by experimental condition** — toggle individual columns for Severity, Peaks, Dimensions, and Frequencies
- **Filter by year range** — narrow results to a specific publication period
- **Sort** — click any column header to sort ascending or descending
- **Export to CSV** — download the currently visible results as a `.csv` file
- **Export to LaTeX** — download up to 4 ready-to-use `.tex` files (one per category), only generated when the selected algorithms have data for that category

---

## Repository Structure

```
.
├── index.html   # Main webpage (layout, styles, all application logic)
├── data.js      # Benchmark data as a base64-encoded string (no raw CSV)
└── README.md
```

> **Note:** The raw CSV data is not stored in plaintext. It is base64-encoded in `data.js` and decoded at runtime by `index.html`. Downloading `index.html` alone yields an empty table — both files must be served together.

---

## Deployment (GitHub Pages)

1. Fork or clone this repository
2. Go to **Settings → Pages**
3. Under *Source*, select `main` branch and `/ (root)` folder
4. Click **Save** — your site will be live at `https://alc1218.github.io/MPB/`

No build step or dependencies are required. The page is fully static.

---

## Adding or Updating Results

To add a new algorithm or correct an existing entry, edit the source CSV and regenerate `data.js`:

```bash
# Encode the CSV to base64 and wrap it as a JS assignment
cat MPB_results.csv | base64 | tr -d '\n' | xargs -I{} echo "window._D='{}';" > data.js
```

Then commit and push both `data.js` and your updated source CSV.

---

## Column Reference

Results are grouped into four experimental conditions:

| Group | Columns | Fixed parameter |
|---|---|---|
| **Severity** | Sev. 0 – Sev. 6 | Peaks = 10, Dim = 5, Freq = 5000 |
| **Peaks** | P = 1, 5, 20, 30, 40, 50, 100, 200 | Sev. = 1, Dim = 5, Freq = 5000 |
| **Dimensions** | D = 10, 20, 50, 100 | Sev. = 1, Peaks = 10, Freq = 5000 |
| **Frequencies** | f = 500, 1000, 2000, 2500, 3000, 10000 | Sev. = 1, Peaks = 10, Dim = 5 |

Each cell displays `mean ± std. error`. A dash (`—`) indicates the condition was not reported in the original paper.

---

## Citation

If you use this collection in your research, please cite the individual papers listed in the table. If you wish to cite the benchmark table itself, please contact us at [mpb_benchmark@gmail.com](mailto:mpb_benchmark@gmail.com).

---

## Contact

Missing an entry? Found an error? Reach out at [mpb_benchmark@gmail.com](mailto:mpb_benchmark@gmail.com).
