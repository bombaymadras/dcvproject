# Mutual fund costs and returns: large-cap equity funds

**DA2402 Data Curation and Visualisation, IIT Madras. Project 11, category: Large Cap.**

Do equity funds with a higher expense ratio return less, and how large is the gap between direct and regular plans?

**Answer.** Across funds, no: the expense ratio does not predict which fund earns more (3-year Direct returns run from 6.3% to 12.7% a year, with a flat TER-vs-return slope, p = 0.61). Within a fund, yes, by exactly the fee: the Regular plan charges a median 1.11 pp a year more than the Direct plan of the same fund (0.18 to 1.66 pp depending on the fund) and returns a median 1.15 pp a year less. Pick the fund by its track record; between similar funds take the cheaper; always buy the Direct plan.

## Open the results

- `clean/dashboard.html`: the full dashboard, opens offline in any browser (double-click). Ten plots, introduction, method, conclusion, limitations, attributions.
- `clean/performance_table.html`: returns since launch, Regular vs Direct, per fund.
- Interactive site with tabs: run the last cell of `notebooks/03_visualisation.ipynb` and open http://127.0.0.1:8050.

## Repository layout

```
raw/        files exactly as downloaded, named by source and month (never edited)
interim/    parsed feeds as Parquet, usable window Oct 2024 to Sep 2026
clean/      largecap_panel.parquet (one row per fund, plan, month), data_dictionary.csv, dashboard.html, plots/
logs/       cleaning_log.csv (one line per step with rows affected) and audit tables
notebooks/  01_collection, 02_cleaning, 03_visualisation
```

## Data

| Source | URL | Collected |
|---|---|---|
| AMFI NAV history (daily NAV, all schemes) | https://portal.amfiindia.com/DownloadNAVHistoryReport_Po.aspx | 24 Oct 2026, one month per request |
| AMFI Total Expense Ratio (monthly, NSDL scheme code) | https://www.amfiindia.com/api/populate-te-rdata-revised (via the `amfipy` wrapper) | 24 Oct 2026 |
| AMFI Fund Performance (1/3/5-year and since-launch returns, AUM) | https://www.amfiindia.com/otherdata/fund-performance, Open Ended · Equity · Large Cap | 24 Oct 2026, by hand |

robots.txt and AMFI's terms of use are checked and quoted in `01_collection.ipynb`. Data used under AMFI's terms for personal, non-commercial purposes; not redistributed.

## Run

Python 3.12 with pandas, numpy, scipy, pyarrow, openpyxl, requests, amfipy, plotly, dash, dash-bootstrap-components, jupyter.

1. `01_collection.ipynb`: downloads 45 months of NAV and TER (about two hours the first time; skipped if the files exist) and writes the interim Parquets.
2. `02_cleaning.ipynb`: name normalisation, name-to-NSDL-code dictionary, large-cap and growth-only slice, join, missing days and NAV-jump checks, monthly panel. Under a minute.
3. `03_visualisation.ipynb`: all plots, the static pages and the Dash site.

## Panel

31 open-ended large-cap funds, growth option, Direct and Regular plans, October 2024 to September 2026: 1,446 fund-plan-months. 35 funds exist in the category: JioBlackRock is excluded on purpose (no Regular TER in two months), ITI and JM are lost because AMFI's TER feed lists them under a second, wrong scheme code in some months, Motilal Oswal because the NAV feed leaves its Plan and Option fields blank.

## Attributions

- Data: Association of Mutual Funds in India (AMFI), as above.
- Software: Python, pandas, NumPy, SciPy, Plotly, Dash, dash-bootstrap-components, openpyxl, pyarrow, requests, amfipy, Jupyter.
- AI tools were used for small pieces of code help. All code was run and checked by the author; every number is computed from the data.

## Contributions

| Member | Work |
|---|---|
| Soham (YOUR FULL NAME, ROLL NUMBER) | 100%: data collection, storage, cleaning, visualisation, site and presentation |
