## RNN and LSTM model for RNA structure predictions

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

