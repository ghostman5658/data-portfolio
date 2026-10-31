# American Childcare Affordability

A plain measure of whether infant center-based care is affordable, and whether a higher burden lines up with mothers leaving the labor force.

## Business question

Is infant center-based care affordable relative to the 7 percent of median household income used as a policy benchmark, and does a higher burden track lower female labor-force participation?

## Who this is for

State and federal staff working on early childhood policy, plus journalists and advocacy groups who need a national figure they can source. The number is a briefing statistic, not a household budget.

## Data

U.S. Department of Labor National Database of Childcare Prices. The file covers roughly 34,500 county-year records from 2008 through 2018. The workbook in this folder is `nationaldatabaseofchildcareprices.xlsx`. The department's technical guide is `technical-guide.pdf`.

The main metric is the annual cost of infant center-based care as a share of median household income. Annual cost is the weekly median times 52. Rows missing price or income are dropped from the ratio. The analytic sample is about 23,600 county-years.

## Methods

This is a descriptive brief, not a predictive model. State medians, a 2008–2018 trend, and a correlation with female labor-force participation carry the argument. Charts anchor the color scale at the 7 percent benchmark so a state cannot look affordable by accident.

`paper.ipynb` and `paper.pdf` are the final write-up. `analysis.ipynb` loads the workbook and builds the figures.

## Results

The median county-year burden is about 16 percent, more than double the benchmark, and more than 99 percent of valid county-years exceed 7 percent. No state clears the line. The correlation with female labor-force participation is about -0.06. Cost alone does not explain whether mothers work.

## Ethics and limitations

- The 7 percent line is a policy heuristic, not a legal standard. Multiplying the weekly median by 52 overstates part-year care.
- County medians hide households. Imputed prices in thin counties add error.
- The file ends in 2018. It does not describe post-pandemic prices.
- A weak correlation is not evidence that cost does not matter.

## How to reproduce

Clone the public repository and run this project from its folder:

https://github.com/ghostman5658/data-portfolio/tree/main/projects/american-childcare-affordability

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open `projects/american-childcare-affordability/analysis.ipynb` and run all cells from that folder. The notebook reads `nationaldatabaseofchildcareprices.xlsx` beside it.
