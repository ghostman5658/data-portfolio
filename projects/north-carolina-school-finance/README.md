# North Carolina School Finance Data Integration

One district-level table from three public files: spending, student outcomes, and child poverty for North Carolina.

## Business question

Once district spending, proficiency, graduation, and child poverty sit on the same row, what actually lines up with student outcomes?

## Who this is for

A district analyst or a state education staffer comparing finance and poverty without rebuilding the join. This is not a school rating and not a causal estimate of spending.

## Data

- NCES F-33 school-district finance survey, fiscal year 2023, in `elsec23.xlsx`.
- Public School Review district pages, cached in `milestone3_scraped.csv` so a rerun does not scrape 116 pages again.
- U.S. Census Bureau SAIPE poverty and income estimates, pulled from the Census API when the notebook runs.

Each source is cleaned, loaded into SQLite, and joined on the NCES district id. The merged table covers 115 North Carolina districts.

## Methods

The notebook reproduces the cleaning, creates `nc_school_districts.db`, and draws the comparison charts from the joined table. The question is whether per-pupil spending, child poverty, and median household income move with math proficiency, reading proficiency, and graduation.

## Results

Per-pupil spending by itself is a weak predictor of math proficiency. Child poverty has a strong negative relationship with graduation rate, and median household income lines up with higher reading proficiency. Lower-income districts rely more on federal and state revenue.

## Ethics and limitations

- The sources are public and district-level. There are no student records in this folder.
- A cross-section cannot say that raising spending would raise proficiency. Poverty, income, and spending move together.
- The Census call needs your own key. Set `CENSUS_API_KEY` before you run the notebook. Request a free key at https://api.census.gov/data/key_signup.html.
- The scraped cache is a snapshot of Public School Review pages. Treat it as a point-in-time extract.

## How to reproduce

Clone the public repository and run this project from its folder:

https://github.com/ghostman5658/data-portfolio/tree/main/projects/north-carolina-school-finance

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export CENSUS_API_KEY=your_key_here
jupyter lab
```

Open `projects/north-carolina-school-finance/analysis.ipynb` and run all cells from that folder. The write-up PDF is [`paper.pdf`](paper.pdf).
