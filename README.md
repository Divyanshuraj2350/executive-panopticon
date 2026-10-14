# Executive Panopticon

A temporal, multimodal risk-monitoring prototype for online examination sessions.
It analyzes eye-gaze measurements, audio levels, and system activity to identify
periods that may deserve human review.

> A flagged episode is a model prediction, not proof of cheating or misconduct.

## Project Objectives

- Analyze behavioral and system signals over time.
- Engineer temporal features from sensor and system-event data.
- Train a Random Forest classifier using a chronological split.
- Identify high-risk episodes and explain predictions using SHAP.
- Export episode-level evidence and review summaries.

## System Workflow

1. Load and inspect sensor and system-event datasets.
2. Align system events using backward as-of matching.
3. Handle missing sensor observations using causal forward filling.
4. Calculate rolling, change, volatility, and range features.
5. Split observations chronologically into training and test sets.
6. Train and evaluate the Random Forest classifier.
7. Identify candidate episodes using a risk threshold of 0.65.
8. Explain predictions with SHAP and export review reports.

## Model Evaluation

| Metric | Recorded result |
|---|---:|
| Accuracy | 0.8180 |
| Precision | 0.9748 |
| Recall | 0.5892 |
| F1-score | 0.7345 |
| ROC-AUC | 0.8331 |
| PR-AUC | 0.8575 |

These are recorded results from the available chronological test split.
They do not establish generalization to other students, exams, or institutions.
The model's predicted probabilities have not been calibrated.

## Episode Review

| Summary metric | Value |
|---|---:|
| Flagged episodes | 13 |
| Low severity | 5 |
| Moderate severity | 4 |
| High severity | 4 |
| Critical severity | 0 |
| Longest episode | 300 seconds |
| Mean episode risk | 0.8372 |
| Recommended for human review | 13 |

Severity categories use a custom score based on mean risk and duration.
They are prototype review priorities, not validated misconduct classifications.

## Explainability

SHAP estimates how features contribute to model predictions. Explanations can
help reviewers understand which signals influenced a prediction.
Feature contributions do not establish causation or prove misconduct.

## Repository Structure

```text
istudio-project/
|-- README.md
|-- data/
|-- notebooks/
|   |-- Executive_Panopticon.ipynb
|   |-- executive_panopticon_all_episode_report.csv
|   |-- executive_panopticon_evidence_report.csv
|   |-- executive_panopticon_review_summary.csv
|-- shap_env/  (local environment)
|-- venv/      (local environment)
```

## Requirements

The current notebook uses Python 3.12 and libraries including pandas, NumPy,
scikit-learn, Matplotlib, seaborn, SHAP, ipywidgets, and Jupyter.

Install the dependencies in an isolated Python environment:

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install pandas numpy scikit-learn matplotlib seaborn shap ipywidgets jupyter
```

## Running the Project

1. Place authorized input datasets in the locations expected by the notebook.
2. Activate the environment and start Jupyter.
3. Open `notebooks/Executive_Panopticon.ipynb`.
4. Run the notebook cells in order.
5. Inspect evaluation results, flagged episodes, charts, and SHAP explanations.
6. Review the generated CSV reports.

Check dataset filenames and paths before running the notebook in a fresh environment.

## Limitations and Responsible Use

- The results come from the available dataset and temporal test split.
- Target labels may not fully represent real-world misconduct.
- Forward filling can preserve stale sensor values during missing intervals.
- Time since the last system event may reflect event timing or data collection.
- Adjacent observations are temporally dependent.
- The custom severity formula and its cutoffs require validation.
- False positives are possible.

Use this prototype for research and human-reviewed analysis only. Before deployment,
validate labels, evaluate independent sessions, assess fairness and privacy,
calibrate probabilities, and establish a review and appeal process.

## Future Improvements

- Validate on independent examination sessions.
- Add probability calibration and threshold-sensitivity analysis.
- Test robustness to missing or stale sensor readings.
- Validate episode grouping and severity cutoffs.
- Build an auditable reviewer dashboard.
- Add automated tests and reproducible environment specifications.

## License

Choose an appropriate license before public distribution and verify that the
datasets and other included materials may legally be shared.
