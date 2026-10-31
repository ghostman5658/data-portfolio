# ZIP Code-Aware Sale Price Prediction

A screening estimate of what a house sold for, using beds, baths, living area, lot size, and ZIP code. The sales run from October 18, 2021, through May 6, 2022. The number is meant to sit beside recent comparable sales. It is not an appraisal, and it is not a number for a loan file.

## Business question

Buyers, sellers, and agents need a price before an offer. A national median is the wrong tool, because an extra bathroom is not worth the same amount in every ZIP. Price the home too high and the deal stalls. Price it too low and the seller leaves money on the table.

Three questions organize the work:

1. How close can beds, baths, living area, lot size, and ZIP get to the observed sale price?
2. How much does ZIP improve on a model that ignores location, and on the ZIP's own median?
3. Does the miss change in California, Texas, and Florida, and in ZIP codes that have only a few sales?

The expectation was that a location-aware model would win, and that error would be higher where a ZIP has few sales.

## Who this is for

The audience is an agent, a buyer, or a seller who wants a range before pulling comps. The useful output is that range, plus a flag when the ZIP does not have enough recent sales to trust a firm number.

## Data

The source is Ahmed Shahriar Sakib's USA Real Estate Dataset on Kaggle, a public compilation of Realtor.com-style listings:

https://www.kaggle.com/datasets/ahmedshahriarsakib/usa-real-estate-dataset

The file has 2,226,382 listing rows. Sold rows are 812,009. Sale dates in the model run from October 18, 2021, through May 6, 2022. Street id is used only to drop duplicate listings. Sale date sets the window and breaks ties. Neither is a predictor.

The file is about 171 MB. It is not stored in git. Download instructions are in [`DATA.md`](DATA.md).

## Preparation

The model keeps documented houses inside a single-family range:

- Drop rows missing price, beds, baths, lot size, living area, ZIP, state, city, or street.
- Keep a valid sale date in 2022 or earlier.
- Keep prices from 10,000 to 5,000,000 dollars.
- Keep 1 to 8 bedrooms and 1 to 8 bathrooms.
- Keep living area from 400 to 8,000 square feet and lots above 0 and up to 10 acres.
- Keep the latest sale for each street and ZIP.

The analysis sample is 594,292 sales. The median home sold for 369,900 dollars, with 1,730 square feet, 3 bedrooms, 2 bathrooms, and 0.19 acres.

That cut is not neutral. About one in four sold rows is missing beds, baths, living area, or lot size. The dropped homes have a median price of about 249,000 dollars. The homes kept have a median of about 370,000 dollars. Florida is a larger share of the dropped rows than of the kept rows. California is the reverse. Every error rate below describes documented houses inside those caps.

## Methods

Sale price has a long right tail, so the models fit the log of price and are scored in dollars. The headline score is the median absolute percent error: the typical miss. Mean absolute error is reported beside it.

Five predictors are compared:

| Predictor | What it uses |
| --- | --- |
| National median | One number from the training sales |
| ZIP median | Median sale price of that ZIP, from training sales only |
| Structure, no location | Beds, baths, log living area, and log lot size |
| Structure plus ZIP | Those features plus the training-set ZIP median |
| Gradient boosting | The same features, with a tree model that can bend the adjustments |

A regression for every ZIP does not fit this file. Only 5,697 of 16,765 training ZIPs have 20 or more sales. One national model with a training-set ZIP median borrows strength, and thin ZIPs stay labeled low-confidence. The linear model stays in the comparison so the bathroom and size adjustments are visible once the ZIP price level is held fixed.

## Validation and results

The holdout is a random 20 percent of cleaned sales, seed 42: 475,282 training rows and 119,010 test rows. Five-fold cross-validation rebuilds the ZIP median inside each training fold, so a home's own sale is never part of the feature used to score it. Homes in a ZIP the training set never saw, 0.7 percent of the test rows, receive the national training median of 369,900 dollars.

Headline metrics use the 104,820 test homes in ZIPs with at least 20 training sales.

| Model | Typical miss | Average dollar miss |
| --- | --- | --- |
| National median | 41.7% | about 261,000 dollars |
| Structure, no location | 36.5% | about 225,000 dollars |
| ZIP median alone | 20.7% | about 144,000 dollars |
| Structure plus ZIP median | 16.2% | about 108,000 dollars |
| Gradient boosting | 14.8% | about 99,000 dollars |

Boosting reaches a dollar R-squared of 0.81. Cross-validation median absolute percent error for boosting averaged 14.7 percent, with a standard deviation of 0.1 point, so the ranking is not one lucky split.

Thin ZIPs are less accurate. Boosting's typical miss is 27.3 percent below 10 training sales, 19.7 percent at 10 to 19, 16.9 percent at 20 to 49, 15.6 percent at 50 to 99, and 13.5 percent at 100 or more. The model still beats the ZIP median in every bin. Below 20 training sales, the recommendation is to withhold a firm number.

On the dense slice, boosting's typical miss is 12.7 percent in California, 15.3 percent in Texas, 16.4 percent in Florida, and 15.2 percent elsewhere. Structure without location misses California by 52.0 percent. The state's median sale in this file is 713,000 dollars, against 315,000 dollars in Texas, so a national anchor is far too low.

About one in three boosted estimates still misses by more than 20 percent. If a list price sits far outside that range, the flag is to pull comps, not to write the point estimate on an offer.

## Key visuals

The canonical notebook draws these figures. Saved copies are in [`figures/`](figures/).

1. Distribution of sale price. Dollar price is right-skewed, which is why the models fit log price.
2. Sale price per square foot by market. California's center sits far above Texas and Florida.
3. Holdout error for the five predictors. Lower is better.
4. Error by how many training sales the ZIP contributed.
5. Error by market for ZIPs with at least 20 training sales.
6. Appendix: boosted predictions against actual sale price. The spread widens as price rises.

## Ethics and limitations

- The ZIP median restates neighborhood price levels, which can track income and older patterns of segregation. Keep it as a comps screen. Keep it out of lending and insurance.
- Dropping incomplete listings changes who is in the model. The published error rates do not describe land, new construction, or a listing that is missing size.
- Condition, year built, and schools are not in the file. That missing information is a large part of why a 15 percent miss remains.
- Rows were not matched to closing statements. Street ids stay anonymized, and the project does not publish address-level records.
- These error rates belong to late 2021 and early 2022. The method can be reused. The 15 percent figure cannot, unless the model is refit on a current sold window.

## How to reproduce

Clone the public repository and run this project from its folder:

https://github.com/ghostman5658/data-portfolio/tree/main/projects/zip-code-sale-price-prediction

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

1. Download `realtor-data.zip.csv`. The steps are in [`DATA.md`](DATA.md). Do not commit that file.
2. Save it in `projects/zip-code-sale-price-prediction/`, next to the notebook.
3. From that folder, run all cells in [`analysis.ipynb`](analysis.ipynb).

The notebook checks `../realtor-data.zip.csv` first, then `realtor-data.zip.csv` in the working directory. Use the second location. The split seed is 42. The same run produces the cleaning counts, the model comparison, the cross-validation, Figures 1 through 5, and the appendix figure.

## Source artifacts

| Artifact | Path |
| --- | --- |
| Repository | https://github.com/ghostman5658/data-portfolio |
| Canonical notebook | [`analysis.ipynb`](analysis.ipynb) |
| White paper PDF | [`paper.pdf`](paper.pdf) |
| Slides | [`slides.pdf`](slides.pdf) |
| Speaker script | [`script.md`](script.md) |
| Figures | [`figures/`](figures/) |
| Data instructions | [`DATA.md`](DATA.md) |
