# Kyle Gotzman

Applied data science portfolio. The page is [https://ghostman5658.github.io/data-portfolio/](https://ghostman5658.github.io/data-portfolio/).

Eight finished projects are in `projects/`. Two later projects are still open. Each folder has a README with the question, the data, the result, and the commands to rerun the notebook.

## Projects

| Project | Folder |
| --- | --- |
| ZIP Code-Aware Sale Price Prediction | [projects/zip-code-sale-price-prediction](projects/zip-code-sale-price-prediction) |
| Telecom Customer Churn Prediction | [projects/telecom-customer-churn](projects/telecom-customer-churn) |
| Tariff Announcement Market-Reaction Classification | [projects/tariff-announcement-market-reaction](projects/tariff-announcement-market-reaction) |
| American Childcare Affordability | [projects/american-childcare-affordability](projects/american-childcare-affordability) |
| North Carolina School Finance Data Integration | [projects/north-carolina-school-finance](projects/north-carolina-school-finance) |
| Combined-Cycle Power Plant Output | [projects/combined-cycle-power-plant](projects/combined-cycle-power-plant) |
| Movie Recommendation System | [projects/movie-recommendation](projects/movie-recommendation) |
| TSA Complaint Trends | [projects/tsa-complaint-trends](projects/tsa-complaint-trends) |

## Run a project

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open the project's `analysis.ipynb` and run it from that folder. The housing listings file is not in git. Download it with `projects/zip-code-sale-price-prediction/DATA.md`. The school-finance notebook reads `CENSUS_API_KEY` from the environment.

## Cleanup

Course-named repositories were not published. This repository is the public copy, organized by subject. See [CLEANUP.md](CLEANUP.md).
