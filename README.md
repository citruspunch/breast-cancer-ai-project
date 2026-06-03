# Breast Cancer Wisconsin Diagnostic Classification

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/your-username/breast-cancer-ai-project/blob/main/notebooks/01_main_pipeline.ipynb)
[![Python 3.11](https://img.shields.io/badge/python-3.11-blue.svg)](https://www.python.org/downloads/release/python-3110/)
[![uv](https://img.shields.io/badge/uv-fast-magenta)](https://github.com/astral-sh/uv)
[![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)](https://github.com/astral-sh/ruff)

## Overview
This repository contains a Clinical Decision Support System (CDSS) prototype designed to classify breast mass aspirations as benign or malignant. Built strictly with classical, highly interpretable machine learning algorithms, this pipeline prioritizes clinical sensitivity over raw accuracy to minimize the risk of false negatives.

> **Academic & Clinical Disclaimer:** > This project was developed as an academic exercise in software engineering and data science. It utilizes a historical dataset from 1995 and does not represent modern, diverse clinical populations. The models herein are for educational research and **do not constitute a diagnostic medical device**. Clinical diagnoses must always be performed by licensed medical professionals.

## Dataset Details
The model is trained on the **Breast Cancer Wisconsin (Diagnostic) Data Set**, originally created by Dr. William H. Wolberg at the University of Wisconsin.
* **Instances:** 569
* **Features:** 30 continuous morphological variables (radius, texture, perimeter, area, smoothness, etc., derived from digitized FNA images).
* **Target:** Malignant (encoded as `1`) vs. Benign (encoded as `0`).
* **Ingestion:** Zero-touch. The data is fetched dynamically via `sklearn.datasets.load_breast_cancer()` to guarantee absolute reproducibility. 

## Architectural Methodology
To ensure strict mathematical validity and prevent data leakage, this pipeline enforces the following engineering constraints:
1. **Target-Aware Multicollinearity Filtering:** Geometrically redundant features (e.g., area and perimeter) are dynamically identified via Pearson correlation ($|r| > 0.85$) and programmatically dropped based on their signal strength to the clinical target.
2. **Zero-Leakage Pipelines:** All data scaling (`StandardScaler`) is strictly encapsulated within `scikit-learn` Pipeline objects to ensure the validation distributions never leak into the training folds during cross-validation.
3. **Stratified Partitioning:** The innate 63/37 benign-to-malignant clinical ratio is mathematically preserved across all Train/Test splits and K-Fold iterations.
4. **Recall Optimization:** Hyperparameter tuning (`GridSearchCV`) is explicitly instructed to maximize `Recall` (Sensitivity) for the Malignant class, penalizing the algorithm heavily for False Negatives.

## Repository Structure

```text
BREAST-CANCER-AI-PROJECT/
├── README.md                   # Project documentation
├── requirements.txt            # Universal dependency fallbacks
├── pyproject.toml              # uv and ruff configuration
│
├── notebooks/                  
│   └── 01_main_pipeline.ipynb  # Primary ML pipeline execution
│
├── outputs/                    
│   ├── figures/                # Visualizations (ROC Curves, Confusion Matrices)
│   └── metrics/                # Exported classification reports
│
└── poster/                     
    └── final_poster.pdf        # Scientific poster presentation (Spanish)
```

## Local Installation & Execution

This project uses [uv](https://github.com/astral-sh/uv) for blazing-fast, deterministic dependency management, but supports standard `pip` for universal accessibility.

### Option A: Using `uv` (Recommended)

Bash

```
# Clone the repository
git clone [https://github.com/your-username/breast-cancer-ai-project.git](https://github.com/your-username/breast-cancer-ai-project.git)
cd breast-cancer-ai-project

# Initialize the environment and install dependencies instantly
uv sync

# Activate the virtual environment
source .venv/bin/activate
```

### Option B: Using Standard `pip`

Bash

```
# Clone the repository
git clone [https://github.com/your-username/breast-cancer-ai-project.git](https://github.com/your-username/breast-cancer-ai-project.git)
cd breast-cancer-ai-project

# Create a standard virtual environment
python -m venv .venv
source .venv/bin/activate

# Install the pinned requirements
pip install -r requirements.txt
```

## Authors

- **Andrés Tobar** - 23001175
    
- **Samuel Marroquín** - _[Teammate 2 Role]_
    
- **Daniel Pérez** - _[Teammate 3 Role]_
    

_Universidad Galileo - [Course Name/Year]_