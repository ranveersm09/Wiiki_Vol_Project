# Does wikipedia attention predict stock volatility?

This is a small data science project to ask a pretty simple question: when people suddenly start reading about a company on Wikipedia, does that tell us anything useful about how volatile that stock is going to be next week? Or is Wikipedia just reacting to news that already happened?

I test that with a proper time-based split, compare it to a basic volatility baseline, and keep volume in the model so Wikipedia has to earn its spot.

## Quick takeaway

In this setup, Wikipedia attention did not add any real predictive power once recent volatility was already in the model.

- same as last week: MAE = 0.00589, R2 = 0.39532
- past volatility only: MAE = 0.00526, R2 = 0.53805
- past volatility + wikipedia: MAE = 0.00526, R2 = 0.53805

So the Wikipedia feature was basically the same as the baseline. It did not improve the forecast.

## What's in the repo

```text
wiki_attention_volatility.ipynb   the full analysis
requirements.txt                 required Python packages
data/                            cached csv files created by the notebook
```

## Data

You do not need to download anything by hand. The notebook fetches and caches the data into `data/`:

| file | what it contains | source |
| --- | --- | --- |
| `prices.csv` | daily open, high, low, close, and volume for big US stocks | Yahoo Finance via `yfinance` |
| `wiki_views.csv` | daily English Wikipedia pageviews for each company page | Wikimedia REST API |
| `universe.csv` | ticker-to-page mapping | written in the notebook |

If you want to force a refresh, just delete one of the CSVs and rerun the notebook.

## How to run

```bash
pip install -r requirements.txt
jupyter lab
```

Or just open the notebook in VS Code and run all cells top to bottom. The first run usually takes a minute or two because it downloads the data.

## What the notebook does

1. Builds a daily volatility measure from the high/low price range
2. Predicts the average volatility over the next 5 trading days
3. Uses recent volatility, return, volume, and Wikipedia attention as features
4. Checks whether Wikipedia attention comes before volatility or after it
5. Uses a walk-forward setup instead of random train/test splits
6. Compares a few different models to see whether Wikipedia adds anything

## Interpretation

The main result is that Wikipedia is not giving us much extra information beyond recent volatility. The model with past volatility alone already does a pretty solid job, and adding Wikipedia does not move the needle at all.

That makes sense with the story that Wikipedia views are probably more of a reaction than a real lead indicator. People often search and read about a company after the news already broke, so the attention spike may be happening after the market has already started pricing it in.

## Things I'd try next

- add earnings dates as a control
- build separate models by stock
- compare with Google Trends or Reddit attention
- use a richer model like a random forest or gradient boosting
- test different time splits to see whether the result is stable
