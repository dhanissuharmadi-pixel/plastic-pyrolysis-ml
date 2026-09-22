# AI-assisted plastic pyrolysis yield prediction

Predicting pyrolysis product yields (liquid / gas / solid) from feedstock and
operating conditions, so you can reason about which conditions maximize a
desired product from waste plastic.

## Why

Plastic waste can be thermally cracked (pyrolysis) into liquid hydrocarbons,
gas, and char. The split between those three depends on plastic type,
temperature, residence time, heating rate, catalyst, and reactor setup. Instead
of trial-and-error, this project learns the mapping from published experiments
and uses it to point at promising conditions.

This mirrors a well-established approach in the literature: on this kind of small
tabular data, gradient boosting (XGBoost) tends to win, and SHAP is used to
explain which conditions drive each fraction.

## The question

> Can a model predict pyrolysis product yields from operating conditions across
> different plastic feedstocks, and tell us which conditions matter most?

## Data

Rows are individual experiments compiled by hand from published studies. See
[`data/schema.md`](data/schema.md) for every column, unit, and allowed value.

- `data/dataset_template.csv` - the empty template you fill from papers.
- `data/example_synthetic.csv` - generated fake data so the pipeline runs before
  real data exists. **Not real. Do not report results from it.** The notebook
  prints a loud warning when it uses this file.

The notebook loads `dataset_template.csv` if it has real rows, otherwise falls
back to the synthetic set (and warns).

`data/reference/` holds real public datasets pulled as reference (CC BY 4.0),
with sources and licenses in [`data/reference/REFERENCES.md`](data/reference/REFERENCES.md).
These are source material for building the real dataset, not yet in the project
schema.

## Method

- Three separate regressors, one per fraction (liquid / gas / solid).
- Compare RandomForest, XGBoost, and LightGBM. XGBoost is the expected winner but
  the comparison is part of the result.
- k-fold cross-validation (the dataset is small, so no single train/test split).
  Report R², MAE, RMSE as mean ± std.
- SHAP for interpretability: which conditions push each fraction up or down.
- Predictions from the three models are normalized to sum to 100 %.

## What makes this different

Most yield-prediction projects stop at "conditions -> yield". This one adds two
things on top:

- **Condition recommender (inverse optimization).** Ask it the reverse question:
  `recommend_conditions("PP", "max_liquid")` searches the condition space and
  returns the best temperature / catalyst / residence time for a target product,
  restricted to conditions the model has actually seen.
- **Trust-aware predictions.** Every prediction carries a conformal `±` band
  (validated to ~90 % coverage) and an in-domain / extrapolating flag, so the
  model tells you when it is guessing instead of pretending it always knows.

One line: a recommender that knows when it is guessing.

## Run

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/pyrolysis_yield.ipynb
```

Run the notebook top to bottom. It generates the synthetic data (if needed),
compares models, shows SHAP, builds the conformal + in-domain trust layer, runs
the condition recommender, and saves everything to `models/`.

## Status

Early but complete end to end on synthetic demo data: model comparison, SHAP,
trust layer (conformal ~90 % coverage + in-domain flag), and the recommender all
run. Next step is collecting real rows from the literature into
`dataset_template.csv`; the notebook reruns unchanged on real data once you do.

## Roadmap ideas (later)

- Streamlit demo app (pick plastic type + conditions, see predicted yields live).
- A short writeup framing it as research (problem, data, method, results, limits).
