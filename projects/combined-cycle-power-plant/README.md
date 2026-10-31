# Combined-Cycle Power Plant Output

Which ambient conditions drive net electrical output, and do those conditions fall into distinct operating regimes?

## Business question

Plant operators need to know what to watch. Ambient temperature, exhaust vacuum, ambient pressure, and relative humidity are the candidates. The useful output is which ones dominate, and whether the plant separates into low, medium, and high output regimes.

## Who this is for

Operators and analysts at a combined-cycle plant. The file is one plant from 2006 through 2011. It is not a claim about every plant.

## Data

UCI Machine Learning Repository, Combined Cycle Power Plant. Nearly 10,000 full-load hourly observations. The Excel file is `data/CCPP/Folds5x2_pp.xlsx`. The UCI readme is beside it.

| Column | Meaning | Unit |
| --- | --- | --- |
| AT | Ambient temperature | °C |
| V | Exhaust vacuum | cm Hg |
| AP | Ambient pressure | mbar |
| RH | Relative humidity | % |
| PE | Net hourly electrical energy output | MW |

There were no missing values. A few outliers in pressure and humidity were standardized rather than dropped.

## Methods

Correlation, linear regression, a log-output check, logistic regression of high versus low output, k-means with three clusters, and a random forest. The narrative is [`narrative.md`](narrative.md).

## Results

Temperature and exhaust vacuum dominate output, with correlations about -0.95 and -0.87. Linear regression reaches an R-squared near 0.93. Random forest improves that to about 0.96, with RMSE about 3.31 MW. K-means separates low, medium, and high output regimes. For prediction, the forest is the better model. For a coefficient someone can read, the linear model is the one to show.

## Ethics and limitations

- One plant, full load, 2006 through 2011. Do not treat the coefficients as a control law for a different unit.
- Temperature and vacuum move together, so a story that credits only one of them overstates how separate they are.
- The clusters describe this file. They are not a labeled operating procedure.

## How to reproduce

Clone the public repository and run this project from its folder:

https://github.com/ghostman5658/data-portfolio/tree/main/projects/combined-cycle-power-plant

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open `projects/combined-cycle-power-plant/analysis.ipynb` and run all cells from that folder. The notebook reads `data/CCPP/Folds5x2_pp.xlsx`.
