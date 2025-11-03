# ⚡ A Causality-Based Trading Strategy in European Energy Markets

**Author:** Julius Gruber
**Date:** November 2025
**Institution:** Baruch College, Master of Financial Engineering
**Advisor:** Prof. Andrew Lesniewski
**Document:** [Capstone_Project_Julius_Gruber.pdf](reports/Capstone_Project_Julius_Gruber.pdf)

---

## 🧭 Abstract

This repository accompanies the capstone thesis *“A Causality-Based Trading Strategy in European Energy Markets”* (Gruber, 2025).
The study investigates the integration of **causal discovery algorithms** with **quantitative forecasting models** to predict returns and construct **market-neutral trading strategies** in **European energy and gas futures markets**.

Unlike traditional financial assets, energy commodities are shaped by physical constraints, regulatory frameworks, and geopolitical shocks.
This project applies the **PCMCI (Peter and Clark Momentary Conditional Independence)** algorithm — a modern causal discovery approach for time series — and contrasts it with traditional econometric techniques such as **Granger Causality**.
The empirical analysis demonstrates that causal methods can extract predictive signals from complex inter-market dependencies and translate them into statistically and economically significant trading performance.

---

## 🎯 Research Objectives

The central research questions addressed in the accompanying paper are:

1. **Can causal discovery algorithms identify stable, predictive relationships** among energy futures returns across European markets?
2. **Do causal features yield improved forecast accuracy** relative to benchmark econometric models such as VAR and Granger causality?
3. **Can causal forecasts be monetized** through systematic, market-neutral portfolio construction under realistic transaction costs?

The investigation is positioned at the intersection of **causal inference**, **time-series econometrics**, and **quantitative trading**.

---

## 🧩 Methodological Framework

### 1. Data

* **Source:** Bloomberg — daily on-the-run futures contracts (back-adjusted for roll).
* **Markets:** German Power, Dutch Power, Dutch Gas, German Gas, Polish Power, Austrian Power, Czech Power, French Power.
* **Time Period:** Multi-year panel split into 3-year training and 1-year out-of-sample backtesting.
* **Transformations:** Log returns for stationarity, clipping of outliers, standardization.

### 2. Causal Discovery Algorithms

* **Granger Causality:** Baseline method estimating vector autoregressive (VAR) models to test predictive directionality.
* **PCMCI:** Combines constraint-based causal graph estimation with lag-specific momentary conditional independence testing.

  * Controls for confounding and autocorrelation.
  * Scales efficiently to high-dimensional datasets.
  * Implemented via the [Salesforce *CausalAI* library](https://opensource.salesforce.com/causalai/latest/) or `tigramite`.

### 3. Forecasting Model

A linear **Ordinary Least Squares (OLS)** model is trained on the causally-selected features:

[
Y_{t+1} = S_t \beta + \epsilon_{t+1}
]

where (S_t) denotes the subset of predictors identified by PCMCI.

### 4. Backtest Methodology

The trading simulation follows the five-step procedure described in Section 4.4.1 of the thesis:

1. Estimate causal parents for each contract.
2. Train the linear model on the in-sample segment.
3. Predict next-day returns out-of-sample.
4. Enter **long** (if forecast > 0) or **short** (if forecast < 0) positions.
5. Normalize weights daily to maintain market neutrality and include a **1 bp transaction cost**.

Performance is evaluated using **Sharpe ratio**, **maximum drawdown**, and **directional accuracy**.

---

## 📊 Empirical Results

Two backtests were conducted as outlined in Sections 4.3–4.4 of the document:

| Dataset              | Markets Included                           | Accuracy (%) | Key Observations                                           |
| -------------------- | ------------------------------------------ | ------------ | ---------------------------------------------------------- |
| **Small Dataset**    | GER Energy, DUT Energy, DUT Gas, GER Power | 47–59        | Predictability observed in energy, but not gas.            |
| **Extended Dataset** | Adds POL, AUT, CZE, FRA Energy             | 50–64        | Strongest predictive power for Austrian and Dutch markets. |

**Portfolio Performance (Market-Neutral):**

* **Sharpe Ratio:** 1.7
* **Max Drawdown:** 17.5 %
* **Transaction Costs:** 1 bp included

These results confirm that **causal inference adds explanatory power** beyond traditional lag-based predictors. Gas markets remain harder to model, likely due to omitted exogenous variables (e.g., weather, LNG flows, storage levels).

---

## 📁 Repository Structure

```text
energy-causal-trading/
│
├── README.md
├── requirements.txt
├── notebooks/
│   └── causal_energy_trading.ipynb        # Main analysis notebook
├── src/
│   └── energy_causal_trading/
│       ├── data_loader.py
│       ├── preprocessing.py
│       ├── causal.py
│       ├── backtest.py
│       └── plotting.py
├── reports/
│   └── Capstone_Project_Julius_Gruber.pdf
└── data/
    ├── raw/ (not tracked)
    └── processed/ (not tracked)
```

---

## ⚙️ Installation & Reproduction

### 1. Environment Setup

```bash
conda create -n causal_ai_env python=3.9
conda activate causal_ai_env
pip install -r requirements.txt
```

### 2. (Optional) Install CausalAI

```bash
pip install causalai
```

### 3. Run the notebook

Open `notebooks/causal_energy_trading.ipynb` in **Jupyter Lab** or **VS Code**, execute all cells, and reproduce the empirical findings of the thesis.

---

## 🧠 Key Insights

* **Causal discovery** isolates stable predictive mechanisms rather than coincidental correlations.
* **PCMCI** provides superior robustness and interpretability compared to traditional Granger tests in high-dimensional energy markets.
* **Causal-based trading strategies** yield consistent, market-neutral performance even under realistic costs.
* **Future extensions** should incorporate exogenous macro- and weather-driven factors to improve gas-market forecasts.

---

## 🧾 Citation

If referencing this repository or the underlying document, please cite as:

> Gruber, J. (2025). *A Causality-Based Trading Strategy in European Energy Markets.*
> Master’s Capstone Project, Baruch College, New York.
> [https://github.com/<your-username>/energy-causal-trading](https://github.com/)

BibTeX:

```bibtex
@misc{gruber2025causality,
  author       = {Gruber, Julius},
  title        = {A Causality-Based Trading Strategy in European Energy Markets},
  year         = {2025},
  howpublished = {\url{https://github.com/<your-username>/energy-causal-trading}},
  note         = {Capstone Project, Baruch College MFE Program}
}
```

---

## 📚 References

* Runge, J. (2022). *Discovering Contemporaneous and Lagged Causal Relations in Autocorrelated Nonlinear Time Series.* arXiv: 2003.03685.
* Oliveira, D. C., et al. (2024). *Causality-Inspired Models for Financial Time Series Forecasting.* arXiv 2408.09960.
* Molak, A., & Jaokar, A. (2023). *Causal Inference and Discovery in Python.* Packt Publishing.
* Kristensen, A. (2024). *Why Has TTF Become the European Wholesale Natural Gas Benchmark?* Time2Market Blog.

---

## 🪐 License

This repository is made available for **academic and educational purposes**.
Use, modification, and distribution are permitted under the terms specified in the repository’s license file.
Commercial applications require prior written consent from the author.

