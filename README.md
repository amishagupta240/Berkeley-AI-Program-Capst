# Formula 1 Scheduling Trends: Can History Predict the Future Calendar?

**A data science capstone project — no prior data science background required to read this.**

📓 Technical notebooks: [`notebooks/01_eda.ipynb`](notebooks/01_eda.ipynb) · [`notebooks/02_modeling.ipynb`](notebooks/02_modeling.ipynb)

---

## The Problem

Formula 1 is a billion-dollar global business, and every race weekend sets off a chain reaction: hotels book up, flights spike in price, host cities stretch their infrastructure, and broadcasters lock in contracts months in advance. None of that works well if the calendar is a guess.

**This project asks:** *Can historical F1 race data be used to identify scheduling trends and predict future season calendars or popular circuit selections?*

Specifically, we wanted answers to three practical questions a race promoter, sponsor, or city official might actually ask:

1. How many races should we expect on next season's calendar?
2. Is our circuit likely to be invited back next year, or are we at risk of being dropped?
3. Are there natural "tiers" of circuits (e.g., historic anchors vs. newer additions) that behave differently?

## The Data

We used the [Kaggle Formula 1 World Championship dataset](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020), which covers every F1 race from 1950 through 2024 — 1,125 races across 77 circuits in 35 countries. For this scheduling-focused analysis we used three of the dataset's tables: race-by-race records, circuit details (location, country), and season lists. (The full Kaggle download also includes lap times, pit stops, and driver/constructor results — those weren't relevant to a scheduling question, so they're not included in this repo.)

## What We Found

**1. The calendar has grown almost every decade — and it's not slowing down.**
F1 ran about 7-8 races a season in the 1950s; it's running 22-24 a season in the 2020s. 2020 is a clear one-off dip (the pandemic-shortened season), not a reversal of the trend.

**2. The growth is happening almost entirely outside Europe.**
Europe hosted essentially the entire calendar before 1990. Its *share* has fallen to roughly half today — not because Europe is losing races, but because nearly all new growth (Bahrain, China, Singapore, Azerbaijan, Saudi Arabia, Qatar, and others) is happening in Asia and the Middle East. Anyone planning around F1's economic footprint should be watching those regions, not assuming Europe's historical dominance continues.

**3. A small core of circuits carries the calendar; everyone else rotates.**
Monza, Monaco, Silverstone, and Spa-Francorchamps have hosted races almost every year for 70+ years. Meanwhile, a much larger group of circuits — often in newer markets — have appeared for only a season or two. If you're a newer host city, history says you're more likely to be a "tryout" than a permanent fixture unless you build a longer track record.

**4. F1 has visibly globalized, season by season.**
The number of distinct countries and continents on the calendar each year has climbed steadily since the 1980s, reflecting a deliberate global expansion strategy rather than organic growth in any one place.

## What the Models Predict

| Question | Approach | Result | What it means |
|---|---|---|---|
| How many races next season? | Regression (Linear Regression, Random Forest, Gradient Boosting) + a dedicated time-series method (Holt's linear trend) | Forecasts land around **22-24 races** for the next several seasons, with the model typically within ~1-1.5 races of the true number in past holdout tests | The calendar's growth is slowing into a plateau — consistent with F1's own public statements about a self-imposed ~24-race ceiling |
| Will a specific circuit be retained next season? | Classification (Logistic Regression, Random Forest) | Random Forest correctly identifies some circuits that go on to be dropped; Logistic Regression tends to just predict "retained" for everyone | For a real early-warning use case, Random Forest is the more useful of the two, even though both score similarly on paper |
| Do circuits fall into natural tiers? | Clustering (K-Means) | Four clear groups: **legacy anchors** (Monaco, Monza-type), **long-running international staples** (e.g., Brazil, Australia), **short-lived European tryouts**, and **recent expansion circuits** (mostly post-2000s, concentrated in Asia/the Middle East) | Gives a simple way to talk about circuit risk — a brand-new circuit in the "recent expansion" cluster is playing a fundamentally different game than a 60-year veteran, even if this season's numbers look similar |

Full methodology, code, evaluation metrics, and charts are in the notebooks linked at the top of this file.

## Important Limitations

- The dataset only covers 75 seasons — not a lot of history for machine learning, especially for the circuit-retention model.
- **The biggest calendar decisions (which circuit gets added or dropped) are driven by commercial contracts, geopolitics, and promoter negotiations — none of which are in this dataset.** These models describe *historical patterns*, not the actual decision-making process. They're best used to sanity-check or inform planning, not to replace it.
- The 2020 season (COVID-19) is a known outlier and was flagged rather than treated as a "normal" data point.

## Recommendations / Next Steps

1. **Track the plateau, not just the growth.** Planners assuming F1 will keep adding races indefinitely should revisit that assumption — the data suggests a ceiling near 24 races.
2. **Watch Asia and the Middle East for the next wave of host cities**, not Europe — that's where the last two decades of growth actually happened.
3. **New host cities should plan for a multi-year "tryout" period**, not assume a single successful race weekend guarantees a long-term slot.
4. **Future work:** if contract/commercial data (race hosting fees, contract lengths) becomes available, combining it with this schedule data would meaningfully improve the circuit-retention model, since that's the piece currently missing.

## Repository Structure

```
f1-capstone/
├── README.md                  ← you are here
├── requirements.txt           ← Python packages needed to run the notebooks
├── data/
│   ├── races.csv               (race-by-race records, 1950-2024)
│   ├── circuits.csv            (circuit name, location, country)
│   └── seasons.csv             (season list)
└── notebooks/
    ├── 01_eda.ipynb            ← exploratory analysis: growth, regional shift, circuit popularity
    └── 02_modeling.ipynb       ← regression forecasting, classification, clustering
```

## How to Run This Yourself

```bash
pip install -r requirements.txt
jupyter notebook notebooks/01_eda.ipynb
```

Run `01_eda.ipynb` first — `02_modeling.ipynb` rebuilds the same cleaned dataset independently, so the notebooks can also be run in either order.

## Data Source & License

Data: [Formula 1 World Championship (1950-2024)](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020) on Kaggle, compiled from the [Ergast Developer API](http://ergast.com/mrd/). Used here for educational purposes as part of a data science capstone project.
