# Replicating Card (1995), "Using Geographic Variation in College Proximity to Estimate the Return to Schooling"

ECON 5200 midterm project: a replication study, with robustness checks the original did not run.

## The paper and its headline claim

- **Paper:** Card, D. (1995). "Using Geographic Variation in College Proximity to Estimate the Return to Schooling." In L. N. Christofides, E. K. Grant and R. Swidinsky (eds.), *Aspects of Labour Market Behaviour: Essays in Honour of John Vanderkamp*, pp. 201–222. Toronto: University of Toronto Press.
- **Identification strategy:** instrumental variables. Growing up near a four-year college in 1966 (`nearc4`) instruments years of schooling (`educ`), with log hourly wage in 1976 (`lwage`) as the outcome.
- **Headline claim:** the 2SLS return to a year of schooling is **0.132** (SE 0.055), Table 3, Panel A, column 5, against an OLS estimate of 0.075 (Table 2, column 2).
- **My replication:** **0.1315** (SE 0.0550), a gap of **−0.38%**, as `python reproduce.py` prints it. The full theirs-vs-mine table, with seven estimates, is `output/headline_table.csv`.

## Reproduce the headline table

Python 3.11 or newer (tested on 3.14.10). From the repository root:

```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python reproduce.py
```

`python reproduce.py` is the one command. It downloads the data into `data/` if it is not already there, runs `notebooks/01_positioning_and_replication.ipynb` and then `notebooks/02_robustness.ipynb` top to bottom in a fresh kernel, and prints the headline table from `output/headline_table.csv`. Executed copies of the notebooks, with their outputs, go to `output/executed/`.

- **Expected runtime:** about 10 seconds once the packages are installed (measured on a MacBook, both notebooks included). The first run adds a few seconds to download the 0.5 MB data file.

## Data: source and licence

- **Source:** `card.dta` from Scott Cunningham's *Causal Inference: The Mixtape* repository, https://raw.githubusercontent.com/scunning1975/mixtape/master/card.dta. The underlying data are the National Longitudinal Survey of Young Men (NLSYM), 1966 and 1976 waves, as assembled by Card. 3,010 men after dropping missing values.
- **Included or downloaded:** downloaded, not committed. `download_data.py` fetches the file on the first run, and `.gitignore` excludes `data/*.dta`. The mixtape repository permits copies under an MIT-style licence, but downloading keeps the repository small and makes the source of the data explicit.
- **Note:** the file contains three derived columns (`u_lwage`, `lwage_hat`, `u_educ`) that are not Card's variables. The notebooks do not use them.

## Repository map

| Path | What it is |
|---|---|
| `reproduce.py` | The one command: runs the notebooks in name order and prints the headline table |
| `download_data.py` | Data URLs. `MY_PAPER = "card"`, and `python download_data.py` fetches `card.dta` into `data/` |
| `notebooks/01_positioning_and_replication.ipynb` | Sections 1 and 2: positioning notes, the replication of Card's Tables 2 and 3, the theirs-vs-mine table, the discrepancy log. **No AI.** |
| `notebooks/02_robustness.ipynb` | Section 3: four robustness exercises (instrument strength, Anderson–Rubin intervals, alternative specifications, an exclusion-restriction placebo). AI used, logged in `ai-appendix.md` |
| `output/headline_table.csv` | The theirs-vs-mine table, written by notebook 01 |
| `output/replication_table.csv` | The full replication table with standard errors, written by notebook 01 |
| `output/` | Every table and figure the report uses, written by the notebooks |
| `report/` | The report PDF |
| `ai-appendix.md` | P.R.I.M.E. entries and full transcripts for Sections 3 to 6, and the no-AI statement for Sections 1 and 2 |
| `AUDIT.md` | How to run the peer audit filed on a classmate's repository |
| `requirements.txt` | Pinned package versions |

## Use of AI

Sections 1 and 2 were completed without AI.

AI (Claude) was used as a documented co-pilot in:

- **Section 3**, the four robustness exercises in `notebooks/02_robustness.ipynb`: adapting the course starter's functions, building the exercise code, and explaining the econometrics. The "What it implies" paragraphs are my own. P.R.I.M.E. entries and the full transcript are in [`ai-appendix.md`](ai-appendix.md).
- **Section 6**, the repository: help setting up git, fixing the notebook kernel, and drafting this README. This is disclosed as a short entry in [`ai-appendix.md`](ai-appendix.md).
