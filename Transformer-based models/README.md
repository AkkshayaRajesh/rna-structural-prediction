Project Structure Overview

This project predicts RNA secondary structure using Transformer-based models and pretrained nucleotide language models.

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

Folders
data/

All datasets used in the experiments.

archivell/processed/
Cleaned CSV files for the ArchiveII dataset: train / val / test splits and a file converted from bpseq.

archivell/raw/
Original raw files before preprocessing (not used directly by the model).

bprna_nf15/
Folder reserved for the bpRNA-NF1.5 dataset (not fully used yet).

results/

Saved checkpoints and logs
(e.g., best_transformer.pt, best_transformer_weighted.pt).

rna-structural-prediction-main/ and rna-structural-prediction-main.zip

Original starter code and archive from the homework/project.
Our modified code lives in src/, so this folder is mostly kept for reference.

src/

All main Python code for our experiments.

src/models/ – Model definitions

__init__.py
Makes the models folder a Python package and re-exports classes so we can write from .models import RNATransformer.

transformer.py

RNATransformerConfig: small config class (vocab size, d_model, dropout, max_len).

RNATransformer: Transformer encoder that takes tokenized RNA sequences and outputs a pairwise base-pair probability matrix.

PairwiseBilinearHead: bilinear head that converts per-position hidden states into an 
(
𝐿
×
𝐿
)
(L×L) score matrix.

pretrained_rna.py

PretrainedRNAConfig: config for using a HuggingFace pretrained model.

PretrainedRNAPairwise: wraps a pretrained nucleotide/DNA model (e.g., DNABERT-2) and adds the same bilinear pairwise head.

Used when we run with --model_type pretrained.

Other src/ files

data_utils.py

BASE2IDX and encode_sequence: map RNA bases A/C/G/U to integer IDs.

dotbracket_to_pairs and pairs_to_matrix: convert dot-bracket structures to a symmetric 
(
𝐿
×
𝐿
)
(L×L) label matrix.

ArchiveIIDataset: torch.utils.data.Dataset that reads archivell_*.csv and returns (x, mask, y, length, seq_id, seq, db).

archive_collate_fn: custom collate function that pads sequences and label matrices inside a batch.

decoder.py

Nussinov-style dynamic programming decoder.

Takes the predicted pairwise scores and the RNA sequence, and returns a dot-bracket structure (no pseudoknots).

Used only for qualitative demos at test time.

debug_decode_example.py
Small script to test the decoder end-to-end on a single example.
Helpful for debugging Nussinov and dot-bracket conversion.

train_transformer.py
Main training script for:

a transformer trained from scratch (--model_type transformer), and

a pretrained encoder + bilinear head (--model_type pretrained).

It handles:

argument parsing (paths, hyperparameters, threshold)

dataset / dataloader setup

training loop with standard BCE loss

validation and test metrics (PPV, recall, F1, true/pred pair rate)

saving the best checkpoint

Nussinov decoding demo on the first test sample

train_transformer_weighted.py
Variant of the training script that uses class-weighted BCE to handle strong class imbalance (few positive base pairs).
The rest of the pipeline (data, model, metrics, decoder) is the same as in train_transformer.py, but the loss function gives more weight to positive pairs.

This structure lets us:

reuse the same dataset, decoder, and evaluation code

swap between scratch Transformer and pretrained LLM encoder

compare vanilla vs. class-weighted training in a clean way
