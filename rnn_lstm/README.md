## RNN and LSTM model for RNA structure predictions

### Description

This project is a comparative benchmark of Recurrent Neural Networks (RNN/GRU) and Long Short-Term Memory (LSTM) architectures for the task of RNA secondary structure prediction. It focuses on training these models to predict a base-pairing probability matrix from a raw RNA sequence. The core methodology involves using a Nussinov Dynamic Programming decoder to convert the probabilistic output into a biologically valid, non-crossing structural prediction, followed by evaluation using standard pair-level metrics like Precision, Recall, and F1-Score on the ArchiveII and bpRNA datasets.


### Installation and Execution

Create a conda environment with all the required packages installed.

```
conda create -n rna-env python=3.10 pytorch torchvision torchaudio -c pytorch -c conda-forge numpy pandas scikit-learn tqdm -y
conda activate rna-env
```

Execute single python file to load the dataset, train the two models and evaluate the models.
```
python run_all_experiments.py
```
Output evaluation metrics are saved as `results.csv`


### File structure
```
├── all_results.csv
├── data
│   ├── archivell
│   │   ├── Test.csv
│   │   ├── Train.csv
│   │   └── archiveII_from_bpseq.csv
│   └── bprna
│       ├── Test.csv
│       ├── Train.csv
│       └── bpRNA-NF-15.0.csv
├── dataset.py
├── decoder.py
├── evaluate.py
├── lstm_archivell.pt
├── lstm_bprna.pt
├── results.csv
├── rnn_archivell.pt
├── rnn_bprna.pt
├── rnn_model.pt
├── run_all_experiments.py
├── train_lstm.py
└── train_rnn.py
```

- `data/` : contains two dataset used in the model training. (The same Train, Test split are used for Transformer-based models.)
- `dataset.py` : codes for data loader
- `decoder.py` : codes for decoder
- `evaluate.py` : codes for computing the evaluation metrics
- `results.csv` : output file containing evaluation metrics
- `train_lstm.py` : define biLSTM architecture
- `train_rnn.py` : define RNN architecture
- `run_all_experiments.py` : codes for tunning the models, and reproduce output file
- `all_results.csv` : result file from training model with limited length of sequences (refer to report!)
