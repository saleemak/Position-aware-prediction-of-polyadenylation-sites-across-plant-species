# Position-aware prediction of polyadenylation sites across plant species

Code and results for the manuscript *"Position-aware prediction of polyadenylation sites across plant species: benchmarking DNA language models, sequence descriptors and positional baselines"* by Saleema K ([ORCID 0009-0003-2217-0799](https://orcid.org/0009-0003-2217-0799)), Department of Computer Science, MES Keveeyam College, Valanchery, India. Submitted to *Plant Gene* (2026).

![Graphical abstract](figures/Graphical_abstract.png)

## What the study does

On the poly(A) tasks of the [Plants Genomic Benchmark](https://huggingface.co/datasets/InstaDeepAI/plant-genomic-benchmark) (*A. thaliana*, *O. sativa* indica and japonica, *M. truncatula*, *T. pratense*, *C. reinhardtii*), it compares eight feature sets with logistic regression and XGBoost:

| Set | Features |
|---|---|
| E0 | One-hot encoding of the 41 bases around the learned signal position (164 features) |
| E1 | k-mer frequencies, k = 3–5 (1,344) |
| E2 | 111 entropy-based and compositional sequence descriptors |
| E3 | Frozen [PlantCaduceus-l20](https://huggingface.co/kuleshov-group/PlantCaduceus_l20) embedding, read at the signal position |
| E4–E7 | E3 + k-mer, E3 + E2, E0 + E2, E0 + E2 + E3 |

Analyses include cross-species transfer (trained on *A. thaliana*), paired bootstrap tests, SHAP, feature-group ablation, motif perturbation, a 6 × 6 train/test matrix and evaluation on the full imbalanced test files.

## Main results

| Result | AUROC |
|---|---|
| k-mer content (E1), mean-pooled embedding | ≈ 0.50 (chance) |
| Embedding read at the signal position ±5 bp (E3) | 0.76 |
| One-hot baseline (E0), *A. thaliana* | 0.867 |
| One-hot + descriptors (E6), trained on each species, mean of six | **0.935** (AgroNT, fine-tuned 1B parameters: 0.937) |

The descriptors' gain comes from positional A+T composition, not from entropy.

## Repository contents

| Path | Contents |
|---|---|
| `notebooks/PlantDNA_LM_PolyA_Kaggle_v10_robustness.ipynb` | Complete pipeline: every result, table and figure in the paper |
| `results/` | All result tables (CSV) from the final run, including per-sequence test predictions (`test_predictions.csv.gz`) |
| `figures/` | Figures 1–9, S1 and the graphical abstract as in the manuscript |
| `requirements.txt` | Python packages (installed automatically by the notebook) |
| `CITATION.cff` | Citation information |

### Key result files

| File | Paper |
|---|---|
| `table2_within_species.csv` | Table 3 |
| `table3_cross_species_auroc.csv` | Table 4, Fig. 5 |
| `table4_fusion_significance.csv` | Table 5 |
| `table4b_vs_onehot_significance.csv` | Table 6 |
| `table_transfer_matrix.csv` | Table 7, Fig. 9 |
| `table_full_test.csv` | Section 3.9 (full imbalanced test files) |
| `table_entropy_ablation.csv` | Fig. 7 |
| `shap_top25.csv`, `shap_block_share.csv` | Fig. 6, Section 3.6 |
| `layer_probe.csv` | Fig. 3 |

## How to run (Kaggle, free GPU)

1. Create a Kaggle notebook, choose **File → Import Notebook** and upload the v10 notebook.
2. Settings: **Accelerator → GPU T4 x2**, **Internet → On**.
3. **Save Version → Save & Run All**. A run from scratch takes about 65 min. The first run also builds the Mamba wheels; the notebook's Part A, step 7, explains how to save them as a Kaggle dataset and skip this step next time.
4. Download `results_bundle.zip` from the **Output** tab. It contains every result table and figure (600 dpi PNG and vector PDF).

All data are downloaded automatically from Hugging Face. Two complete re-runs reproduced every AUROC value exactly (seed 42).

## Licence

Code: GNU General Public License v3.0 (see `LICENSE`). The Plants Genomic Benchmark is distributed by its authors under CC BY-NC-SA 4.0.

## Citation

Please cite the paper (details will be added on publication) or use `CITATION.cff`.
