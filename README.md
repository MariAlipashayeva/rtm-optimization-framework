# rtm-optimization-fra# Requirements Traceability Matrix (RTM) Optimization Framework

An automated, two-phase framework for requirements-to-test traceability synthesis using machine learning feature classification combined with Pareto multi-objective search.

---

## Architecture Overview

The pipeline processes candidate requirement-test pairs through two distinct phases:

1. **Phase X ($X$: LightGBM Feature Classifier)**
   * Extracts lexical (TF-IDF), structural, and temporal process features.
   * Generates continuous candidate probability scores $P(y_{ij} = 1 \mid \mathbf{v}_{ij})$.
   * Filters out spurious candidate pairs below threshold $\tau_{\text{base}}$ to reduce combinatorial complexity.

2. **Phase Y ($Y$: NSGA-II Multi-Objective Optimization)**
   * Operates on the filtered candidate space $P_{\text{filtered}}$.
   * Evaluates competing objective functions: **Precision**, **Recall/Coverage**, and **Inspection Cost**.
   * Output: Non-dominated Pareto frontier ($\mathcal{P}^*$) representing optimal trace link selections ($\mathbf{z}^*$).

---

## Directory Structure & Sample Data

* `data/sample_x_features.json`: Raw candidate pairs with extracted feature vectors ($\mathbf{v}_{ij}$) and output probabilities ($\hat{y}_{ij}$).
* `data/sample_y_optimization.json`: Multi-objective evaluation state, decision vectors ($\mathbf{z}$), and Pareto-optimal solution sets.

---

## Quick Start

```bash
# Clone the repository
git clone [https://github.com/your-username/rtm-optimization.git](https://github.com/your-username/rtm-optimization.git)
cd rtm-optimization

# Install dependencies
pip install -r requirements.txt

# Run candidate filtering (Phase X)
python src/phase_x_classifier.py --input data/sample_x_features.json --threshold 0.35

# Run Pareto optimization (Phase Y)
python src/phase_y_nsga2.py --input data/sample_x_features_filtered.json --pop_size 100 --generations 50
