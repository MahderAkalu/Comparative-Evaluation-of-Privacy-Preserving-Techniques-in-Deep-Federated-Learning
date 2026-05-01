# Comparative Evaluation of Privacy-Preserving Techniques in Federated Deep Learning



## Overview

This repository contains the full experimental codebase for this study:
*"Comparative Evaluation of Privacy-Preserving Techniques in Federated Deep Learning."*

The study provides a systematic, empirical comparison of three privacy-preserving 
federated learning techniques: Federated Averaging (FedAvg), Differential Privacy 
(DP-FL), and Secure Aggregation (SecAgg), which are evaluated across two datasets under 
realistic non-IID data distributions (Dirichlet α = 0.5). Experiments are assessed 
across four dimensions: model utility (Accuracy, AUC-ROC), empirical privacy (membership inference and 
gradient leakage), and communication overhead.


## Repository Structure

```
├── CIFAR_10.ipynb           # Full benchmark notebook for CIFAR-10 (vision task)
├── Shakespeare.ipynb        # Full benchmark notebook for Shakespeare (NLP task)
├── benchmark_results.csv        # Aggregated results across all seeds and methods
├── Images/                      # All plots and figures used in the thesis
│   ├── CIFAR 10 MIA ROC Curve Plot.png
│   ├── CIFAR 10 Accuracy vs Rounds Plot.png
│   ├── CIFAR 10 Privacy Utility Tradeoff Plot.png
│   ├── Shakespeare Accuracy & Perplexity vs Rounds Plot.png
│   ├── Shakespeare Privacy-Utility Tradeoff Plot.png
│   └── Shakespeare Cost Comparison.png
└── README.md
```


## Experimental Setup

| Component        | Details                                                        |
|------------------|----------------------------------------------------------------|
| Datasets         | CIFAR-10 (vision), Shakespeare (NLP)                           |
| Models           | Small CNN (CIFAR-10), LSTM (Shakespeare)                       |
| FL Framework     | Flower (flwr) v1.x                                             |
| DP Library       | Opacus                                                         |
| Methods          | FedAvg (R0), DP-FL σ=0.5 (R1a), DP-FL σ=1.0 (R1b), SecAgg (R2) |
| Clients          | 10                                                             |
| Rounds           | 10                                                             |
| Data Split       | Dirichlet α = 0.5 (non-IID)                                    |
| Seeds            | 42, 123, 7                                                     |
| Hardware         | Intel Arc 140V GPU (16GB VRAM), 32GB RAM                       |

## How to Run

### 1. Open in Google Colab

Each notebook has a direct Colab launch button at the top. Click the badge inside 
the notebook to open it instantly in Colab with no local setup required.

- **CIFAR-10:** [`CIFAR_10.ipynb`](./CIFAR_10.ipynb)
- **Shakespeare:** [`Shakespeare.ipynb`](./Shakespeare.ipynb)


### 2. Run Order

Both notebooks are fully self-contained. Run all cells top to bottom. Each notebook:

1. Installs all required dependencies
2. Downloads or partitions the dataset
3. Runs all four experiments (R0, R1a, R1b, R2) across three seeds
4. Evaluates accuracy, perplexity (Shakespeare only), MIA-AUC, and communication cost
5. Generates and saves all result plots


## Methods

| ID  | Method                  | Description                                              |
|-----|-------------------------|----------------------------------------------------------|
| R0  | FedAvg                  | Standard federated averaging, no privacy mechanism       |
| R1a | DP-FL (σ = 0.5)         | Differential privacy with moderate noise multiplier      |
| R1b | DP-FL (σ = 1.0)         | Differential privacy with high noise multiplier          |
| R2  | SecAgg                  | Secure aggregation with simulated cryptographic masking  |



## Key Results

- **SecAgg** outperforms FedAvg on CIFAR-10 (~21.9% vs ~17.5% final accuracy) while 
  both DP configurations fail to converge beyond random chance (10%) on the vision task.
- **On Shakespeare**, FedAvg, SecAgg, and DP-FL (σ = 0.5) all converge to ~47–48% 
  accuracy, while DP-FL (σ = 1.0) collapses entirely confirming that LSTM training 
  is highly sensitive to strong gradient noise.
- **MIA-AUC** results show DP configurations push inference success toward the 0.50 
  random-guess baseline, while SecAgg retains high utility but remains vulnerable to 
  membership inference.
- A **non-monotonic privacy–utility relationship** is observed: stronger DP noise 
  (σ = 1.0) can outperform moderate noise (σ = 0.5) under non-IID conditions due to 
  implicit regularization effects.

