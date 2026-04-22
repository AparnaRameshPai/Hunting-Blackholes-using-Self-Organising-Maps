# MBH-SOM-UQ

### Uncertainty-Aware Black Hole Parameter Inference via Self-Organizing Maps

![Python](https://img.shields.io/badge/python-3.9+-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-in%20development-orange)

> ⚠️ **This project is currently in active research and development. Code and results will be posted soon.**

---

## Overview

An independent research project inspired by the [HOLESOM paper](https://arxiv.org/abs/2410.11951) (La Torre & Pacucci, 2025). This project extends the original HOLESOM methodology by making uncertainty a first-class citizen of the inference pipeline.

Standard Self-Organizing Maps return a single point estimate — no confidence, no uncertainty. This project asks: **how confident should we actually be in those estimates?**

Given sparse multi-wavelength photometric measurements of a source (radio → X-ray), the pipeline infers two physical parameters of a slowly-accreting massive black hole:

- **M_BH** — black hole mass (solar masses)
- **f_Edd** — Eddington ratio (how fast it is accreting relative to its theoretical maximum)

And crucially, it attaches statistically rigorous uncertainty estimates to both.

---

## Motivation

Slowly-accreting massive black holes are among the hardest objects to detect in the universe. They emit via **Advection Dominated Accretion Flow (ADAF)** — a radiatively inefficient mode that makes them faint and easily confused with other astrophysical sources. HOLESOM introduced a SOM-based approach to identify and characterise them from sparse photometric data.

This project builds on that foundation with a statistical rigour question the original paper leaves open: **are the uncertainties well-calibrated?** Do 90% confidence intervals actually contain the true value 90% of the time? Which uncertainty method is sharper — and when do they disagree, and why?

---

## Methods (Planned)

### Baseline
A Self-Organizing Map trained on ~20,000 synthetic ADAF spectral energy distributions (SEDs), each labelled with known M_BH and f_Edd. Given a new source, the Best Matching Unit (BMU) on the trained grid gives point estimates.

### Method 1 — Monte Carlo Input Perturbation
Propagates photometric measurement errors through the SOM by sampling N perturbed versions of the input within its error bars. The spread of predictions captures **measurement uncertainty**.

### Method 2 — Bootstrap SOM Ensemble
Trains N SOMs on bootstrap resamples of the training data. The spread of predictions across the ensemble captures **model uncertainty** — how sensitive the SOM is to its training sample.

### Evaluation
Both methods evaluated on held-out synthetic SEDs (ground truth known) and validated on Sagittarius A* using:
- Reliability diagrams
- Expected Calibration Error (ECE)
- Continuous Ranked Probability Score (CRPS)
- Interval sharpness comparison

---

## Planned Repository Structure

```
mbh-som-uq/
├── data/
│   ├── generate_seds.py         # synthetic ADAF SED generator
│   └── sgr_a_star.py            # Sgr A* validation source
├── som/
│   ├── train.py                 # SOM training
│   └── predict.py               # BMU lookup, point estimates
├── uq/
│   ├── mc_perturbation.py       # Method 1
│   └── bootstrap_ensemble.py    # Method 2
├── evaluation/
│   ├── calibration.py           # reliability diagrams, ECE
│   └── scoring.py               # CRPS, interval width
├── validation/
│   └── sgr_a_star.py            # real-world validation
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_baseline_som.ipynb
│   ├── 03_uq_methods.ipynb
│   └── 04_results_figures.ipynb
├── results/figures/
└── tests/
```

---

## Stack

- **SOM** — `minisom`
- **Numerics** — `numpy`, `scipy`
- **Evaluation** — `properscoring`, `scikit-learn`
- **Visualisation** — `matplotlib`, `seaborn`

---

## Inspiration & Credit

This project is directly inspired by:

> La Torre, V. & Pacucci, F. (2025). *HOLESOM: Constraining the Properties of Slowly-Accreting Massive Black Holes with Self-Organizing Maps.* The Astrophysical Journal. [arXiv:2410.11951](https://arxiv.org/abs/2410.11951)

All credit for the original HOLESOM methodology goes to the authors. This project is an independent extension focused on uncertainty quantification and calibration.

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Author

Masters student in Data Science, Statistics & Decision Analysis.  
Building this as an independent research project at the intersection of ML, statistics, and astrophysics.
