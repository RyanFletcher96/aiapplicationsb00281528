# SmartLend

SmartLend is an educational machine-learning project for exploring loan-default risk prediction. It uses the Give Me Some Credit dataset to study the end-to-end workflow: understanding a business problem, exploring data, preparing a dataset, and evaluating the considerations that matter in a high-impact lending context.

> **Project status:** This repository currently provides an exploratory notebook and a data-preprocessing pipeline. It does not yet contain a trained prediction model, a lending decision system, or a deployed service. The project is not intended for real credit decisions.

## Contents

- [Project brief](#project-brief)
- [Dataset](#dataset)
- [Repository layout](#repository-layout)
- [Getting started](#getting-started)
- [Working with the project](#working-with-the-project)
- [Preprocessing behavior](#preprocessing-behavior)
- [Tests](#tests)
- [Responsible use and limitations](#responsible-use-and-limitations)

## Project brief

SmartLend is a fictional UK fintech company considering how machine learning could support loan-risk review. The prediction task is binary classification: estimate whether a borrower will experience serious delinquency within two years.

The dataset target is `SeriousDlqin2yrs`:

- `1`: the borrower experienced 90 or more days past due delinquency within two years
- `0`: the borrower did not experience that outcome

The input data includes credit utilisation, age, debt ratio, monthly income, counts of credit lines and late payments, real-estate loans, and number of dependants. The business context, stakeholders, constraints, and initial success criteria are described in [the SmartLend brief](docs/smartlend_brief.md). [The problem-framing worksheet](docs/problem_framing.md) is provided for recording analysis and decisions.

## Dataset

This project uses the **Give Me Some Credit** training dataset. Obtain `cs-training.csv` from the course VLE and place it at:

```text
data/raw/cs-training.csv
```

The raw CSV is intentionally ignored by Git. It is not included in this repository; the original data should be obtained from the course-provided source. The data dictionary supplied with the project is [data/raw/Data Dictionary.xls](data/raw/Data%20Dictionary.xls).

Keep the original CSV unchanged. Perform cleaning and transformations in code, and save derived data under `data/processed/`. The preprocessing script writes its output to `data/processed/cs-processed.csv`.

## Repository layout

```text
smartlend/
├── .github/workflows/ci.yml  # GitHub Actions test workflow
├── config/                   # Project-specific configuration (placeholder)
├── data/
│   ├── raw/                  # Original course data (CSV ignored by Git)
│   └── processed/            # Generated datasets (CSV ignored by Git)
├── docs/
│   ├── eda_*.png             # EDA charts
│   ├── problem_framing.md    # Problem-framing worksheet
│   └── smartlend_brief.md    # Business brief
├── models/                   # Saved models (placeholder; none supplied)
├── notebooks/
│   └── 01_eda.ipynb          # Initial exploratory data analysis
├── src/
│   └── preprocess.py         # Data loading and preprocessing pipeline
├── tests/
│   └── test_preprocess.py    # Preprocessing unit tests
└── requirements.txt          # Python dependencies
```

## Getting started

### Requirements

- The workflow file specifies Python 3.13 for its test job.
- `pip` for installing the project dependencies.
- The course-provided `cs-training.csv` file for running the notebook or preprocessing pipeline.

### Create an environment and install dependencies

Run these commands from the repository root.

**Windows (PowerShell):**

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If the Python launcher is unavailable, use `python -m venv .venv` instead, provided that `python` points to a compatible installation.

**Linux / macOS:**

```bash
python3.13 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

After activating the environment, install the dataset at `data/raw/cs-training.csv` as described above.

## Working with the project

### Explore the data

Open [`notebooks/01_eda.ipynb`](notebooks/01_eda.ipynb) in Jupyter or VS Code and run its cells in order. The notebook inspects the dataset's shape, columns, data types, descriptive statistics, missing values, target distribution, and correlations. It also creates charts saved under `docs/`.

The notebook uses paths relative to its `notebooks/` directory (`../data/` and `../docs/`). If a notebook environment reports that it cannot find the CSV or save a chart, set the notebook's working directory to `notebooks/` before running the cells.

### Preprocess the dataset

From the repository root, with the project environment active and the raw CSV in place, run:

```bash
python src/preprocess.py
```

The script reads `data/raw/cs-training.csv`, applies the preprocessing steps described below, creates `data/processed/` if needed, and writes `data/processed/cs-processed.csv`.

### Dependencies

The packages listed in [`requirements.txt`](requirements.txt) support data manipulation (`pandas`, `numpy`), plotting (`matplotlib`, `seaborn`), notebooks (`jupyter`), machine learning (`scikit-learn`), testing (`pytest`), and configuration (`pyyaml`). The repository currently uses scikit-learn as a dependency but does not yet implement model training.

## Preprocessing behavior

The reusable pipeline is implemented in [`src/preprocess.py`](src/preprocess.py). Its current steps are:

1. Read the supplied CSV.
2. Check that the expected target and feature columns are present.
3. Fill missing `MonthlyIncome` and `NumberOfDependents` values with their respective column medians.
4. Retain rows where `RevolvingUtilizationOfUnsecuredLines` is at most `1.0` and `age` is greater than zero.
5. Save the resulting CSV and return the processed DataFrame.

The filtering rules are initial project choices, not validated lending policy. Review their effect during analysis before using them in later modelling work.

## Tests

Run the current unit tests from the repository root:

```bash
python -m pytest tests/ -v
```

The tests cover median imputation and preservation of the expected columns. A workflow file is included for manually triggered tests, with Python 3.13 specified as its test environment.

## Responsible use and limitations

Credit risk predictions can affect people's access to financial services. Any future modelling work should consider class imbalance, suitable evaluation metrics, calibration and decision thresholds, data leakage, explainability, fairness across relevant groups, privacy, auditability, and the consequences of false positives and false negatives.

The dataset and project are for coursework and experimentation. A model developed here must not be treated as validated for deployment or used to make real lending decisions. Document assumptions and limitations, preserve reproducibility, and seek appropriate legal, compliance, and domain review before any real-world use.
