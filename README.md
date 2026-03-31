# TED Auction Modeling (From Scratch)

This workspace is a notebook-first project inspired by the structure in `auction-fraud-detection`, adapted for TED procurement data and bidder-count multiclass modeling.

## Layout

- `notebooks/`: numbered analysis pipeline (wrangling -> EDA -> features -> modeling -> evaluation)
- `src/auction_project/`: reusable pipeline modules
- `src/main.py`: CLI for wrangling/training/evaluation
- `data/raw`, `data/interim`, `data/processed`: data lifecycle folders
- `results/`, `models/`, `reports/figures/`: outputs

## Quick start

1. Create and activate a Python environment.
2. Install dependencies:
   - `pip install -r requirements.txt`
3. Wrangle raw TED data (same load->clean->save flow as reference project):
   - `python src/main.py wrangle --input "data/raw/TED award 2018-2023.xlsx" --output data/interim/ted_awards_wrangled.csv`
4. Run baseline modeling on wrangled data:
   - `python src/main.py train --input data/interim/ted_awards_wrangled.csv --target NUMBER_OFFERS --output results/metrics.json`

## Launch the project

Use the existing VS Code task:

- Task name: `Run Auction CLI Help`
- Command executed: `python src/main.py --help`

Direct CLI launch alternatives:

- Help/command overview:
   - `python src/main.py --help`
- Wrangle:
   - `python src/main.py wrangle --input "data/raw/TED award 2018-2023.xlsx" --output data/interim/ted_awards_wrangled.csv`
- Train:
   - `python src/main.py train --input data/interim/ted_awards_wrangled.csv --target NUMBER_OFFERS --output results/metrics.json`
- Evaluate:
   - `python src/main.py evaluate --input data/interim/ted_awards_wrangled.csv --target NUMBER_OFFERS --output results/evaluation_metrics.json`

## Train/Evaluate outputs

The `train` and `evaluate` commands now follow a model-ready pipeline:

1. Feature engineering (`engineer_features`)
2. Target class creation (`TARGET_CLASS` from `NUMBER_OFFERS` bins)
3. Feature screening (drops near-empty or near-constant columns)
4. Feature selection (mutual information ranking on encoded features)
5. Model training + evaluation

Default artifacts:

- Metrics: `results/metrics.json`
- Model-ready dataset: `data/processed/ted_awards_model_ready.csv`
- Feature summary: `results/feature_engineering_summary.csv`

Notebook stage artifacts:

- Model comparison (baseline + tuned variants): `results/model_comparison_model_ready.csv`
- Best model metadata for evaluation handoff: `results/best_model_config.json`
- Leakage diagnostics: `results/leakage_scan.csv`
- Evaluation outputs: `results/evaluation_metrics.json`, `results/confusion_matrix.csv`, `results/classification_report.csv`, `results/error_cases.csv`, `results/feature_importance.csv`

Useful options:

- `--max-features` to control top-ranked encoded features used for selection
- `--model-ready-output` to change the model-ready CSV location
- `--feature-summary-output` to change the summary CSV location

## Process flow (notebooks 1-5)

- Full project flowchart (paper-style): `reports/process_flow_notebooks_1_5.md`

## Next customization

- Add CatBoost hyperparameter tuning in `src/auction_project/train.py`.
- Export confusion matrix and class report in `src/auction_project/evaluate.py`.
