# Tariff Announcement Market-Reaction Classification

A classifier for whether a market sector rises or falls on the day of a tariff announcement. The result is a sector screen for investment, procurement, and strategy teams, not a trading system.

## Business question

Tariff announcements do not move every industry the same way. Which market segments are more likely to rise or fall after a trade-policy shock, and does the size of the tariff matter more than the market that announcement lands in?

## Who this is for

Investment teams allocating across sectors, manufacturers watching upstream cost, procurement teams in trade-exposed industries, and strategy teams looking for who gains from a protectionist action.

## Data

Two public series are joined:

- Daily sector ETF returns, with prior-day market context.
- A timeline of major tariff actions from 2018 through 2025.

The prepared table has 362 sector-event rows. The same-day broad-market return was kept out of the features so the model does not see the outcome.

This folder includes `sector_impact.csv`, `tariff_timeline.csv`, and `stock_market_reaction.csv`. The notebook writes `model_ready.csv` when it runs.

## Methods

Logistic regression, random forest, and gradient boosting classify the same-day sector return as positive or negative. The score is macro F1, because a model that only calls the majority direction is not useful when sectors split.

## Validation and results

Random forest was the best model.

| Score | Value |
| --- | --- |
| Cross-validated macro F1 | 0.781 |
| Holdout macro F1 | 0.774 |
| Holdout ROC-AUC | 0.835 |
| Holdout accuracy | 0.795 |

Recent market volatility and sector momentum mattered at least as much as the size of the tariff.

## Ethics and limitations

- The label is the sign of a same-day return. That is a classification target, not a claim that the tariff caused the move.
- A few dozen tariff dates do not support a live trading rule. Transaction costs, overnight gaps, and look-ahead in the timeline are not modeled.
- Sector ETFs are a coarse stand-in for a firm's actual exposure.

## How to reproduce

Clone the public repository and run this project from its folder:

https://github.com/ghostman5658/data-portfolio/tree/main/projects/tariff-announcement-market-reaction

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open `projects/tariff-announcement-market-reaction/analysis.ipynb` and run all cells from that folder. The narrative write-up is [`writeup.md`](writeup.md). The PDF is [`writeup.pdf`](writeup.pdf).
