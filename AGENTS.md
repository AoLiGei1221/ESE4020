# Repository Guidelines

## Project Structure

- `main.ipynb` contains the Dry Bean classification experiment, comparing logistic regression and random forest, and generates the checkpoint plots.
- `data/` holds the UCI Dry Bean dataset in XLSX and ARFF formats and its accompanying documentation. `baseline.txt` records the checkpoint methods and results.
- Notebook runs generate split manifests, tuning scores, and out-of-fold predictions in `results/` and write `baseline.png` at the repository root.

## Development Commands

From the repository root, create a Python 3.12 environment, install the versions documented in the notebook, and launch Jupyter.

```sh
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install notebook numpy==1.26.4 pandas==2.2.2 scipy==1.13.1 \
  scikit-learn==1.4.2 matplotlib==3.8.4 openpyxl==3.1.2 \
  joblib==1.4.2 threadpoolctl==2.2.0
jupyter notebook main.ipynb
```

Run all cells in order. The initial run downloads the UCI ZIP to `data/` if the archive is missing. Later runs can reuse completed metrics. Set `RECOMPUTE = True` to refit the models after changing data, preprocessing, or model settings.

## Coding Style and Naming

Use four-space indentation, `snake_case` for functions and variables, and uppercase constants such as `SEED`. Use `pathlib.Path` for repository-relative paths and preserve `source_row_id` in split and prediction outputs.

## Testing and Reproducibility

Restart the kernel and run all cells with `RECOMPUTE = True` to validate experiment changes. Notebook assertions check the data schema, split disjointness, one out-of-fold prediction per development row, and consistency of reported metrics. For comparable results, keep seed `4020`, the stratified 80/20 split, and five inner and five outer folds. Fit scaling within training folds and reserve the final test set until model choices are frozen.

## Commits and Pull Requests

The Git history uses short subjects such as `add code`. Keep the subject concise and name the change, for example `Check prediction alignment`. In a pull request, summarize what changed and how you verified it. Link any related issue and include updated macro-F1 scores or plots when the change affects them.
