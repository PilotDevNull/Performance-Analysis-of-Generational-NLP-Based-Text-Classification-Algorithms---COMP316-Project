# COMP316 — NLP Text Classification Project
### Performance Analysis of Generational NLP-Based Text Classification Algorithms
**The Steam Curators** | University of KwaZulu-Natal | May 2026

---

## Project Overview

This project empirically compares three generations of NLP sentiment classifiers — Multinomial Naive Bayes, Bidirectional LSTM, and BERT — on binary sentiment classification of 60,000 Steam game reviews collected via the Steam Web API.

| Model | Accuracy | F1-Score | Train Time |
|---|---|---|---|
| Naive Bayes | 84.11% | 83.53% | 0.03s |
| LSTM | 84.05% | 83.61% | 35s |
| BERT | 90.30% | 90.28% | 86 min |

---

## Folder Structure

```
COMP316 Project/
│
├── COMP316_PROJECT_COMPLETE_Combined Notebooks.ipynb   ← All 4 notebooks merged
├── COMP316 Project Report FINAL.pdf                    ← Final submitted report
│
├── Data Used/                  ← Raw and cleaned dataset CSV files
│
├── Individual Notebooks/       ← Standalone notebooks for each model
│
└── Locally Trained Bert Files/ ← Local BERT training output
    └── COMP316/
        ├── .ipynb_checkpoints/
        ├── bert_steam_model/           ← Saved fine-tuned BERT weights
        ├── data/                       ← Data folder used during local training
        ├── bert_confusion_matrix.png   ← BERT confusion matrix plot
        ├── bert_training_history.png   ← BERT training accuracy/loss curves
        ├── bert_metrics.csv            ← BERT evaluation metrics
        ├── bert_predictions.csv        ← BERT predictions on 18,000 test reviews
        ├── bert_steam_FINAL.ipynb      ← Commented BERT notebook (no outputs)
        ├── bert_steam_FINAL COMPLETE.ipynb     ← BERT notebook with outputs
        └── bert_steam_FINAL CgfOMPLETE.ipynb   ← Earlier version (ignore)
```

---

## File Descriptions

### Root Level

| File | Description |
|---|---|
| `COMP316_PROJECT_COMPLETE_Combined Notebooks.ipynb` | Single notebook containing all four sections in order: Data Preprocessing → Naive Bayes → LSTM → BERT. All cell outputs from completed training runs are preserved. |
| `COMP316 Project Report FINAL.pdf` | The final project report submitted for assessment. |

---

### `Data Used/`
Contains the raw and cleaned Steam review datasets used across all three models.

| File | Description |
|---|---|
| `steam_reviews_train_FINAL.csv` | 42,000 training reviews (21,000 positive, 21,000 negative) |
| `steam_reviews_test_NO_SENTIMENT_FINAL.csv` | 18,000 test reviews — no labels (model input) |
| `steam_reviews_test_WITH_SENTIMENT_FINAL.csv` | 18,000 test reviews — with ground truth labels (evaluation) |

> All three models were trained and evaluated on these identical files to ensure a fair comparison.

---

### `Individual Notebooks/`
Standalone `.ipynb` files for each model, with full inline documentation.

| File | Description |
|---|---|
| `COMP316_PROJECT_DATA_FINAL.ipynb` | Data extraction and preprocessing pipeline |
| `naive_bayes_steam_FINAL_COMPLETE.ipynb` | Naive Bayes — TF-IDF vectorisation + MultinomialNB |
| `lstm_steam_FINAL_COMPLETE.ipynb` | Bidirectional LSTM — trained on Google Colab |
| `bert_steam_FINAL_COMPLETE.ipynb` | BERT fine-tuning — trained locally on GTX 1060 |

---

### `Locally Trained Bert Files/COMP316/`
All files generated during local BERT training on the GTX 1060.

| File/Folder | Description |
|---|---|
| `bert_steam_model/` | Saved fine-tuned BERT weights and tokeniser. Can be reloaded with `BertForSequenceClassification.from_pretrained('./bert_steam_model')` without retraining. |
| `data/` | Copy of the data files used during local training |
| `bert_confusion_matrix.png` | Confusion matrix heatmap for BERT on 18,000 test reviews |
| `bert_training_history.png` | Training accuracy and loss plotted across 3 epochs |
| `bert_metrics.csv` | Summary metrics: Accuracy, Precision, Recall, F1, training time, inference time |
| `bert_predictions.csv` | Full prediction output: review_id, predicted_sentiment, true_sentiment for all 18,000 test reviews |
| `bert_steam_FINAL COMPLETE.ipynb` | **Use this one** — BERT notebook with full comments and training outputs |
| `bert_steam_FINAL.ipynb` | Commented notebook without outputs (pre-run version) |
| `bert_steam_FINAL CgfOMPLETE.ipynb` | Earlier version — ignore |

---

## How to Run

### Combined Notebook (Recommended)
Open `COMP316_PROJECT_COMPLETE_Combined Notebooks.ipynb` in Jupyter or Google Colab. All outputs are already present — no re-running is required for viewing results.

### Individual Models

**Naive Bayes and LSTM** — run on Google Colab:
1. Upload the notebook and the three CSV files from `Data Used/` to Google Drive
2. Update `DATA_PATH` in the notebook to match your Drive folder
3. Runtime → Run All

**BERT** — run locally on a CUDA-enabled GPU:
```bash
# Install dependencies
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
pip install transformers pandas scikit-learn matplotlib seaborn jupyter

# Verify GPU
python -c "import torch; print(torch.cuda.is_available())"

# Launch Jupyter
jupyter notebook
```
Update `DATA_PATH` in the Configuration cell to point to your local data folder, then run all cells.

> ⚠️ BERT training takes approximately **2–3 hours** on a GTX 1060. The saved model in `bert_steam_model/` can be loaded directly to skip retraining.

---

## Reloading the Saved BERT Model

To run inference without retraining:

```python
from transformers import BertForSequenceClassification, BertTokenizer
import torch

model     = BertForSequenceClassification.from_pretrained('./bert_steam_model')
tokenizer = BertTokenizer.from_pretrained('./bert_steam_model')
model.eval()
```

---

## Group Members

| Name | Student Number | Model |
|---|---|---|
| Jawad Kachbal | 223037045 | BERT |
| Joshua Blom | 224039813 | LSTM |
| Mahomed Valli Mahomed | 224005878 | Naive Bayes |
