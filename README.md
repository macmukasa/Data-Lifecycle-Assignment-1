# Automobile price driver analysis

This repository contains an exploratory data analysis investigating the mechanical, dimensional, and categorical factors that determine car prices. The study evaluates candidate predictors through correlation analysis, hypothesis testing, and variance explained metrics to identify an effective set of pricing variables.

## Dataset

The analysis begins with `automobileEDA.csv`, which contains 201 rows and 29 columns. Each record describes a single automobile model. The target variable is `price`. 

Candidate predictors are separated into 11 continuous variables and 10 categorical attributes. The continuous predictors are `wheel-base`, `height`, `curb-weight`, `engine-size`, `bore`, `stroke`, `compression-ratio`, `horsepower`, `peak-rpm`, `city-mpg`, and `highway-mpg`. The categorical predictors are `symboling`, `make`, `aspiration`, `num-of-doors`, `body-style`, `drive-wheels`, `engine-location`, `engine-type`, `num-of-cylinders`, and `fuel-system`.

Seven columns from the raw file are excluded from analysis. `normalized-losses` is omitted because it reflects insurance risk rather than vehicle pricing. Six derived attributes (`length`, `width`, `city-L/100km`, `horsepower-binned`, `diesel`, and `gas`) are dropped because they duplicate existing measurements.

The cleaning pipeline outputs `Mukasa_Data.csv`, which has 200 rows and 23 columns. The pipeline fills missing values in `stroke` (4 entries) with the column median, caps continuous variables at their 1.5 IQR fences, converts categorical columns to strings, and removes one duplicate row.

## Methods

- Outlier capping at 1.5 IQR fences and median imputation for missing continuous values.
- Continuous relationship evaluation using Pearson correlation coefficients and Spearman rank correlation coefficients.
- Categorical group comparisons using one-way ANOVA to compute effect size via eta squared (η²), supported by Kruskal-Wallis tests.
- Cross-categorical relationship testing using chi-square tests of independence with Cramér's V, followed by two-way ANOVA to evaluate main effects and interaction terms on price.
- Predictor ranking on a shared scale of price variance explained, using r² from linear regression F-tests for continuous predictors and η² from ANOVA for categorical predictors.

## Key findings

The top ten predictors ranked by share of price variance explained are listed below.

| Predictor | Type | F-statistic | p-value | Variance explained (r² or η²) | Rank |
| :--- | :--- | :--- | :--- | :--- | :--- |
| make | categorical | 33.7487 | 0.0000 | 0.7900 | 1 |
| curb-weight | continuous | 641.1092 | 0.0000 | 0.7640 | 2 |
| engine-size | continuous | 525.3237 | 0.0000 | 0.7263 | 3 |
| horsepower | continuous | 376.2339 | 0.0000 | 0.6552 | 4 |
| highway-mpg | continuous | 247.7699 | 0.0000 | 0.5558 | 5 |
| num-of-cylinders | categorical | 62.7215 | 0.0000 | 0.5427 | 6 |
| city-mpg | continuous | 210.6446 | 0.0000 | 0.5155 | 7 |
| drive-wheels | categorical | 81.0553 | 0.0000 | 0.4514 | 8 |
| fuel-system | categorical | 27.7615 | 0.0000 | 0.4195 | 9 |
| wheel-base | continuous | 136.4198 | 0.0000 | 0.4079 | 10 |

Inventory pricing recommendations focus on four non-redundant variables: make, curb weight, horsepower, and drive wheels. Heavier vehicles and vehicles with higher horsepower command higher market prices, while rear-wheel drive carries a clear premium over front-wheel drive. Manufacturer badge shifts baseline valuation, with premium marques such as Jaguar and Mercedes-Benz priced well above volume producers like Chevrolet. Because correlation does not imply causation, adding weight to an existing chassis does not create market value on its own. A practical valuation workflow establishes a baseline from make and drivetrain, adds increments for vehicle mass and engine output, and adjusts for physical condition.

Several groups in the dataset have sparse counts that limit inference. Mercury, twelve-cylinder, and three-cylinder categories contain only one car each. Four-wheel drive hatchbacks as well as front-wheel drive hardtops and convertibles also contain only single vehicles. Continuous predictors exhibit strong multicollinearity, such as curb weight with engine size at 0.87, and city-mpg with highway-mpg at 0.97.

## Figures

The repository analysis produces visual summaries that can be saved into an `images/` folder.

- `images/top_10_predictors.png`: horizontal bar chart comparing the top ten attributes by variance explained.
- `images/correlation_matrix.png`: correlation heatmap for continuous predictors and price.

These image files are not pre-generated in the repository. They can be exported by inserting `plt.savefig("images/<filename>.png", bbox_inches="tight")` immediately before `plt.show()` in cells 48 and 60 of `Assignment1_Mukasa.ipynb`.

## How to run

Running the analysis requires Python 3.14 (or compatible Python 3.9+) and Jupyter.

Install dependencies:
```bash
pip install -r requirements.txt
```

Launch the notebook:
```bash
jupyter notebook Assignment1_Mukasa.ipynb
```

Ensure `automobileEDA.csv` is located in the working directory alongside the notebook before executing cells.

## Repository structure

```text
.
├── .gitignore               # Excludes virtual environments, cache files, and build artifacts
├── Assignment1_Mukasa.ipynb # Primary analysis notebook containing cleaning, EDA, and statistical tests
├── Mukasa_Data.csv          # Cleaned dataset output produced by cell 19 of the notebook
├── README.md                # Project documentation and summary of findings
├── automobileEDA.csv        # Raw automobile dataset with 201 records and 29 columns
└── requirements.txt         # Project dependencies and environment specification
```

## Author

- Author: Mark Mukasa
- Course: Data Science Lifecycle, Assignment 1

