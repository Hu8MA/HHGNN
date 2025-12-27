# H²GNN: Hierarchical Hypergraph Neural Networks for Multi-Type Drug-Drug Interaction Prediction

A hierarchical hypergraph-based deep learning framework for predicting drug-drug interactions (DDIs) using only SMILES strings as input. The model combines chemical and metabolic network information in a two-stage architecture that is simple, efficient, and highly effective.

## 📋 Table of Contents

- [Overview](#overview)
- [Key Achievements](#key-achievements)
- [Repository Structure](#repository-structure)
- [Code Availability](#code-availability)
- [Experimental Summary](#experimental-summary)
- [Results](#results)
- [Comparison with State-of-the-Art](#comparison-with-state-of-the-art)
- [Computational Efficiency](#computational-efficiency)
- [Ablation Studies](#ablation-studies)
- [Reproducibility](#reproducibility)
- [Citation](#citation)
- [Contact](#contact)

## Overview

This repository provides documentation and a structural guide for **H²GNN** (Hierarchical Hypergraph Neural Networks), designed to predict drug-drug interactions. The framework leverages two complementary network stages:

1. **Chemical Network (Stage 1)**: Binary classification of drug interactions using K-mer representations of SMILES strings
2. **Metabolic Network (Stage 2)**: Multi-class classification (86 interaction types) using embeddings transferred from the chemical network

The hypergraph structure enables modeling of higher-order relationships between drugs, capturing complex interaction patterns that traditional pairwise graph-based methods may miss.

## Key Achievements

| Achievement | Value |
|-------------|:-----:|
| 🎯 **Binary Classification ROC-AUC** | **98.46%** |
| 🎯 **Multi-class Classification ROC-AUC** | **99.44%** |
| 🎯 **Top-3 Accuracy (86 classes)** | **98.11%** |
| ⚡ **Training Time (Metabolic Network)** | **18.6 minutes** |
| 💾 **RAM Usage** | **< 0.5 GB** |
| 🔄 **Recovery of Unseen Interaction Types** | **94.7%** |
| 📈 **Improvement over Best Baseline** | **+5% Accuracy** |
| 🖥️ **Hardware Required** | **CPU only (2-core)** |

## Repository Structure

The complete codebase is organized as follows:

```
H2GNN/
│
├── chemical/
│   ├── Best-Experiment-1/          # Top performing experiment (code + results)
│   ├── Best-Experiment-2/          # Second best experiment (code + results)
│   ├── DataSet/                    # Chemical interaction datasets
│   ├── DataSet-Partitions/         # Train/validation/test splits
│   ├── Hypergraph_Chemical/        # Hypergraph construction files
│   └── K-mer/                      # K-mer feature extraction
│
├── metabolic/
│   ├── Best-Embedding/             # Optimal drug embeddings from chemical network
│   ├── Best-Experiment-1/          # Top performing experiment (code + results)
│   ├── Best-Experiment-2/          # Second best experiment (code + results)
│   ├── DataSet-Partitions/         # Train/validation/test splits
│   └── Metabolic-Hypergraph/       # Hypergraph construction files
│
└── baselines/                      # Baseline comparison models
    └── [GCN, GAT, GraphSAGE, XGBoost, Random Forest]/               # Each baseline with its own Colab notebook
```

### Folder Descriptions

| Folder | Type | Description |
|--------|------|-------------|
| `Best-Experiment-1/`, `Best-Experiment-2/` | Code + Results | Google Colab notebooks with outputs for top configurations |
| `DataSet/`, `DataSet-Partitions/` | Data | Raw data and pre-split datasets for reproducible evaluation |
| `Hypergraph_Chemical/`, `Metabolic-Hypergraph/` | Data | Hypergraph structure files |
| `K-mer/` | Data | Pre-computed K-mer features |
| `Best-Embedding/` | Data | Chemical network embeddings for transfer learning |
| `baselines/` | Code + Results | Baseline models with comparison benchmarks |

**Note**: Each experiment folder contains all conducted experiments and their results, not just the top performers.

## Code Availability

All code is implemented in **Google Colab** format (`.ipynb`) to ensure:
- ✅ Easy reproducibility without local setup
- ✅ Clear documentation with inline results
- ✅ Step-by-step execution
- ✅ Accessible on Google Colab Free Tier

**Access Status**: 
> 🔒 The complete code and datasets are currently maintained in a **private repository** and will be made publicly available upon acceptance of the accompanying research paper.

For early access requests, please contact the corresponding author (see [Contact](#contact)).

## Experimental Summary

### Chemical Network
- **Task**: Binary classification (interaction / no interaction)
- **Total Experiments**: 36 configurations
- **Variables**: K-mer lengths, dataset seeds, model architectures

### Metabolic Network
- **Task**: Multi-class classification (86 interaction types)
- **Total Experiments**: 12 configurations
- **Variables**: Chemical embeddings, dataset seeds, model architectures

## Results

### Chemical Network — Binary Classification

#### Top 3 Performing Models

| Rank | Accuracy | F1-Score | ROC-AUC | PR-AUC |
|:----:|:--------:|:--------:|:-------:|:------:|
| 🥇 | 93.20% | 93.41% | **98.46%** | **98.47%** |
| 🥈 | 93.13% | 93.30% | 98.42% | 98.41% |
| 🥉 | 93.01% | 93.30% | 98.41% | 98.40% |

#### Key Findings
- Consistent high performance across all top configurations
- Model maintains stable performance regardless of drug interaction frequency
- Successfully learns representations for both common and rare interacting compounds

---

### Metabolic Network — Multi-class Classification (86 Types)

#### Top 3 Performing Models

| Rank | ROC-AUC | PR-AUC | Top-1 Accuracy | Top-3 Accuracy | Weighted F1 |
|:----:|:-------:|:------:|:--------------:|:--------------:|:-----------:|
| 🥇 | **99.44%** | 87.24% | 85.73% | **98.11%** | **86%** |
| 🥈 | **99.44%** | 87.24% | 85.73% | **98.11%** | **86%** |
| 🥉 | 99.44% | 86.76% | 85.33% | 97.93% | 85% |

#### Key Findings
- Outstanding discrimination ability with 99.44% ROC-AUC across 86 interaction types
- Near-perfect Top-3 accuracy (98.11%) — clinically relevant for decision support
- Most classes achieved F1 scores exceeding 0.85
- Strong diagonal dominance in confusion matrix indicates effective learning despite severe class imbalance

---

### Performance Highlights

| Metric | Chemical Network | Metabolic Network |
|--------|:----------------:|:-----------------:|
| **Best ROC-AUC** | 98.46% | 99.44% |
| **Best Accuracy** | 93.20% | 85.73% (Top-1), 98.11% (Top-3) |
| **Best F1-Score** | 93.41% | 86% (Weighted) |

## Comparison with State-of-the-Art

### Binary Classification (Chemical Network)

| Model | Accuracy | F1 | ROC-AUC | PR-AUC |
|-------|:--------:|:--:|:-------:|:------:|
| Random Forest + Morgan FP | 79% | 80% | 88% | 88% |
| GCN + Morgan FP | 88% | 88% | 95% | 95% |
| **H²GNN (Ours)** | **93%** | **93%** | **98%** | **98%** |

✅ **+5% improvement** in accuracy and F1 over best baseline

### Multi-class Classification (Metabolic Network)

| Model | Top-1 Acc | Top-3 Acc | ROC-AUC | PR-AUC | Weighted F1 |
|-------|:---------:|:---------:|:-------:|:------:|:-----------:|
| GCN + Morgan FP | 45.9% | 80.7% | 97.89% | 56.77% | 48% |
| XGBoost + Morgan FP | 69.9% | 92.4% | 98.46% | 70.92% | 69% |
| GraphSAGE + Morgan FP | 80.9% | 96.8% | 99.41% | 76.76% | 81% |
| **H²GNN (Ours)** | **85.7%** | **98.1%** | **99.44%** | **87.24%** | **86%** |

✅ **+4.8% Top-1 accuracy** and **+1.3% Top-3 accuracy** over best baseline (GraphSAGE)

### Key Advantages Over Baselines
- **No external fingerprints required** — uses only SMILES strings
- **Outperforms models with additional molecular features**
- **Captures higher-order drug relationships** via hypergraph structure

## Computational Efficiency

| Metric | Chemical Network | Metabolic Network |
|--------|:----------------:|:-----------------:|
| **Total Training Time** | 142 min | **18.6 min** |
| **RAM Usage** | 0.46 GB | 0.29 GB |
| **Hardware** | CPU (2-core) | CPU (2-core) |

### Training Time Comparison (Metabolic Network)

| Model | Training Time | Hardware |
|-------|:-------------:|:--------:|
| **H²GNN (Ours)** | **18.6 min** | CPU |
| GCN (2-layer) | 75.6 min | GPU (T4) |
| GraphSAGE (2-layer) | 148.5 min | GPU (T4) |
| XGBoost | 225.6 min | CPU |

✅ **4-12× faster** than baselines while achieving superior accuracy  
✅ **No GPU required** — runs efficiently on CPU

*Note: Single-layer GCN/GraphSAGE on CPU failed to converge. Baselines required deeper architectures and GPU hardware.*

## Ablation Studies

| Ablation | Impact |
|----------|--------|
| **Without Chemical Embeddings** | Performance dropped significantly — validates hierarchical transfer learning |
| **Reversed Attention Flow** | Major performance decline — confirms edge-centric attention is optimal for DDI |
| **Modified Class Weighting** | Trade-off observed — current strategy achieves best balance |

### Generalization Capability

The model demonstrated remarkable generalization by recovering **94.7%** of intentionally excluded interaction types within top-3 predictions — despite never seeing them during training. This suggests H²GNN captures underlying multi-mechanistic pharmacological relationships.

## Reproducibility

To ensure full reproducibility:

- ✅ **Fixed Random Seeds** for all dataset splits
- ✅ **Google Colab Notebooks** with inline outputs
- ✅ **Pre-computed Features** provided
- ✅ **Exact Data Partitions** included

Upon public release:
```
1. Open any experiment notebook in Google Colab
2. Run all cells sequentially
3. Reproduce the reported results
```

## Citation

If you use this work in your research, please cite:

```bibtex
@article{mahdi2025h2gnn,
  title={H²GNN: Hierarchical Hypergraph Neural Networks for Multi-Type Drug-Drug Interaction Prediction},
  author={Hussein Mahdi and Sura Al-Rashid},
  journal={[Journal Name]},
  year={2025}
}
```

*Citation details will be updated upon publication.*

## Contact

For questions, collaborations, or early access requests:

- **Corresponding Author**: Hussein Mahdi
- **Email**: inf787.hussien.a@student.uobabylon.edu.iq
- **Institution**: University of Babylon, Iraq

## Acknowledgements

The author gratefully acknowledges the **DrugBank Foundation** for providing access to the curated drug information resources that supported this study.

---

**Repository Status**: 🔒 Private (pending paper acceptance)  
**Paper Status**: Under review

---

*This README serves as a public guide to the repository structure and results. The complete codebase and datasets will be released upon paper acceptance.*
