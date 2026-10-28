# Arabic Quranic Text Diacritization — CATT Transformer

**Arabic NLP · PyTorch · Transformer Fine-Tuning · Gradio · Telegram Bot**

This project adapts an **existing CATT (Character-based Arabic Tashkeel Transformer)** implementation to Arabic diacritization with **Quranic text data**. It includes a fine-tuning experiment, an interactive **Gradio** interface, and a **Telegram bot** for accessing diacritization functionality.

> **Attribution:** CATT and its original architecture were developed by their original authors. This repository documents my adaptation, fine-tuning experiment, and application integrations—not a Transformer architecture designed from scratch. Add the original CATT paper and repository links here after confirming the exact source used.

## Project highlights

- Prepared input–target pairs by removing diacritics from fully diacritized Arabic text.
- Initialized an existing `TashkeelModel` using a pretrained checkpoint.
- Ran a **50-epoch** PyTorch fine-tuning experiment on the first **2,000** data records with Adam (`lr=1e-5`).
- Built a **Gradio** interface for submitting Arabic text and viewing diacritized output.
- Built a **Telegram bot** interface for Arabic text diacritization.
- Inspected predictions through accuracy calculations and a confusion matrix.

## Example

| Input (without diacritics) | Target (with diacritics) |
| --- | --- |
| بسم الله الرحمن الرحيم | بِسْمِ اللَّهِ الرَّحْمَٰنِ الرَّحِيمِ |

This is a dataset example from the notebook, **not** a guaranteed live-model prediction.

## Training workflow

1. Read `Arabic-Original.csv` as pipe-separated lines and select the diacritized text field.
2. Create an undiacritized input and corresponding diacritized target.
3. Load the pretrained model checkpoint from `models/best_eo_mlm_ns_epoch_193.pt`.
4. Fine-tune with token-level cross-entropy loss for 50 epochs on up to 2,000 records.
5. Save the resulting model state as `best_model.pt`.
6. Inspect accuracy and a confusion matrix using records from the same working dataset.

**Notebook:** [`project.ipynb`](project.ipynb)

## Reported experimental results

| Metric | Notebook output | Important qualification |
| --- | --- | --- |
| Epochs | 50 | As configured in the notebook |
| Records selected | First 2,000 | A truncated working subset, not an independently split test set |
| Sum of per-sample training losses | 4,388.29 → 992.28 | Epoch 1 to epoch 50; **not** mean loss |
| Token-position accuracy | **82.35%** | Calculated on the **first 100 records of the working/training dataset**, not held-out test accuracy |
| Confusion matrix | 6 displayed label indices | Calculated on the first 200 working/training records |

**Evaluation limitation:** These numbers are exploratory in-sample measurements. They are **not** validated generalization performance, Quranic diacritization error rate (DER), or word error rate (WER). The notebook's label extraction handles individual combining marks, and truncates outputs and targets to the shorter sequence; this can misalign character-level targets. A dedicated train/validation/test split and aligned tokenizer labels are needed before reporting reliable benchmark metrics.

## Repository files

| File | Purpose |
| --- | --- |
| `project.ipynb` | Preprocessing, fine-tuning, exploratory evaluation |
| `app.py` | Gradio application |
| `bot.py` | Telegram bot integration |
| `tashkeel_tokenizer.py` | Arabic diacritization tokenizer |
| `transformer.py`, `eo*.py`, `ed*.py`, `utils.py` | Model implementation components |
| `Arabic-Original.csv` | Input dataset; review the source and distribution rights before reuse |
| `models/`, `best_model.pt` | Model weights; may require Git LFS |
| `project.pdf` | Notebook export/report |
| `requirements.txt` | Project dependencies |

## Setup

```bash
 git clone https://github.com/YoussefHelmy22/CATT-Character-based-Arabic-Tashkeel-Transformer.git
 cd CATT-Character-based-Arabic-Tashkeel-Transformer
 pip install -r requirements.txt
```

The project may require **Python 3.10**, compatible PyTorch versions, and Git LFS for model files. Check the existing `requirements.txt` and checkpoint paths before running. To launch the Gradio application, if configured by `app.py`:

```bash
python app.py
```

To use the Telegram bot, provide its token securely through an environment variable or secret manager, following the configuration in `bot.py`. **Never commit Telegram tokens or other secrets.**

## Next improvements

- Validate dataset licensing and Quranic text normalization.
- Fix the target-label alignment for combining marks and special characters.
- Create independent train/validation/test splits and report DER/WER.
- Track validation metrics per epoch and select the best checkpoint by validation performance.
- Add tested startup commands, screenshots, and a demo for Gradio and Telegram.
- Add exact attribution to the original CATT implementation and pretrained checkpoint.

## My contribution

Adapted and fine-tuned an existing CATT implementation using Quranic data, and developed Gradio and Telegram interfaces. The underlying CATT model was not authored from scratch in this project.
