## Project Structure

This project predicts RNA secondary structure using Transformer-based models and pretrained nucleotide language models.

```text
project/
├── data/
│   ├── archivell/
│   │   ├── processed/
│   │   │   ├── archivell_train.csv
│   │   │   ├── archivell_val.csv
│   │   │   ├── archivell_test.csv
│   │   │   └── archivell_from_bpseq.csv
│   │   └── raw/
│   ├── bprna_nf15/
│   └── ...
├── results/
├── rna-structural-prediction-main/
├── src/
│   ├── models/
│   │   ├── __init__.py
│   │   ├── transformer.py
│   │   └── pretrained_rna.py
│   ├── __init__.py
│   ├── data_utils.py
│   ├── decoder.py
│   ├── debug_decode_example.py
│   ├── train_transformer.py
│   └── train_transformer_weighted.py
└── rna-structural-prediction-main.zip
```text


## Folders

### `data/`

All datasets used in the experiments.

- **`data/archivell/processed/`**  
  Cleaned CSV files for the ArchiveII dataset: train / val / test splits and a file converted from bpseq.

- **`data/archivell/raw/`**  
  Original raw files before preprocessing (not used directly by the model).

- **`data/bprna_nf15/`**  
  Folder reserved for the bpRNA-NF1.5 dataset (optional / future use).

---

### `results/`

Saved checkpoints and logs  
(e.g., `best_transformer.pt`, `best_transformer_weighted.pt`).

---

### `rna-structural-prediction-main/` and `rna-structural-prediction-main.zip`

Original starter code and archive from the homework/project.  
Our modified code lives in `src/`, so this folder is kept only for reference.

---

## `src/` – main code

### `src/models/` – model definitions

- **`transformer.py`**
  - `RNATransformerConfig`: config class (vocab size, `d_model`, dropout, `max_len`).
  - `RNATransformer`: Transformer encoder over RNA sequences that outputs a pairwise base-pair probability matrix.
  - `PairwiseBilinearHead`: bilinear head that converts hidden states into an \((L \times L)\) score matrix.

- **`pretrained_rna.py`**
  - `PretrainedRNAConfig`: configuration for using a HuggingFace pretrained nucleotide model.
  - `PretrainedRNAPairwise`: wraps a pretrained encoder (e.g., DNABERT-2) and adds the same bilinear pairwise head.

- **`__init__.py`**  
  Makes the `models` folder a package and re-exports key classes.

---

### Other files in `src/`

- **`data_utils.py`**
  - Base encoding (`A/C/G/U →` integer IDs).
  - Dot-bracket utilities: convert dot-bracket to base-pair lists and label matrices.
  - `ArchiveIIDataset`: PyTorch `Dataset` that reads `archivell_*.csv` and returns  
    `(x, mask, y, length, seq_id, seq_str, dot_bracket)`.
  - `archive_collate_fn`: custom collate function that pads sequences and pairwise matrices.

- **`decoder.py`**
  - Nussinov-style dynamic programming decoder.
  - Converts model pairwise scores back into a dot-bracket structure (no pseudoknots).
  - Used mainly for qualitative decoding demos.

- **`debug_decode_example.py`**  
  Small script to test the decoder on a single example for debugging.

- **`train_transformer.py`**  
  Main training script for:
  - a Transformer trained from scratch (`--model_type transformer`), and  
  - a pretrained encoder + bilinear head (`--model_type pretrained`).

  Handles:
  - argument parsing (paths, hyperparameters, threshold)  
  - data loading  
  - training with standard BCE loss  
  - validation/test metrics (PPV, recall, F1, true/predicted pair rate)  
  - checkpointing the best model  
  - Nussinov decoding demo for the first test sample.

- **`train_transformer_weighted.py`**  
  Variant of the training script that uses **class-weighted BCE** to address strong class imbalance (very few positive base pairs).  
  Shares the same data pipeline, models, metrics, and decoder as `train_transformer.py`.
