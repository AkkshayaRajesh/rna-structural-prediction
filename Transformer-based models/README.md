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
