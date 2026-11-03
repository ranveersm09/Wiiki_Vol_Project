# does wikipedia attention predict stock volatility?

small data science project. the question: when people suddenly start reading about a company on wikipedia, does that tell us anything about how wild the stock will be over the next week? or is wikipedia just reacting to news that already moved the price?

i test it properly with walk forward (no random splits), against the standard HAR volatility model, and with volume as a control so wikipedia has to earn its spot.

## what's in the repo

```
wiki_attention_volatility.ipynb   the whole project
requirements.txt
data/                             created when you run the notebook (cached csvs)
```

## data

nothing to download by hand, the notebook fetches and caches everything into `data/`:

| file | what | source |
| --- | --- | --- |
| `prices.csv` | daily open / high / low / close / volume for 26 big US stocks, 2017 onwards | yahoo finance via `yfinance` |
| `wiki_views.csv` | daily english wikipedia pageviews (humans only) for each company's article | wikimedia REST api |
| `universe.csv` | ticker to wikipedia article title mapping | written by hand in the notebook |

delete a csv in `data/` if you want it refetched.

## how to run

```
pip install -r requirements.txt
jupyter lab   # or just open the notebook in vs code
```

run all cells top to bottom. first run takes a minute or two because of the downloads. there are dropdowns and sliders in a few places (ipywidgets) and the charts are plotly so you can hover and zoom.

## what it does

1. builds a daily volatility measure from high / low prices (parkinson estimator)
2. target is the average variance over the next 5 trading days
3. features: past vol (HAR), 5 day return, volume surge, and two wikipedia attention features
4. lead lag check: does attention come before vol or after it?
5. walk forward: train on everything before each year, test on that year
6. compares HAR, HAR + controls, HAR + controls + wiki, and a gradient boosting model

## results

_fill this in after you run it. put the R2 vs HAR table here and one sentence on what it means._

## things i'd do next

- add earnings dates as a control
- per stock models
- google trends / reddit as other attention sources
- intraday data for a cleaner vol target
