# Running Notebooks in Google Colab

This guide walks through setting up and running any notebook in this repository using Google Colab with a T4 GPU.

---

## 1. Set the Runtime to T4 GPU

Before running any cells:

1. In Colab, go to **Runtime > Change runtime type**
2. Under **Hardware accelerator**, select **T4 GPU**
3. Click **Save**

---

## 2. Store Your Kaggle Credentials as Colab Secrets

Colab Secrets keeps your API key out of notebook output and cell history.

1. Go to **Tools > Secrets** (key icon in the left sidebar)
2. Click **+ Add new secret**
3. Set **Name** to `KAGGLE_KEY` and paste your Kaggle API token (the value of `"key"` from your `kaggle.json`) as the **Value**
4. Add a second secret: **Name** = `KAGGLE_USERNAME`, **Value** = your Kaggle username (the value of `"username"` from your `kaggle.json`)
5. Toggle **Notebook access** on for both secrets

> You can find your credentials at https://www.kaggle.com/settings → **API** → **Create New Token**. This downloads `kaggle.json`.

---

## 3. Clone the Repository

Run this in a Colab cell to clone the repo and navigate into it:

```python
!git clone https://github.com/DigiCrome-Academy/image-classification-bnelsonemail-1.git
%cd image-classification-bnelsonemail-1
```

---

## 4. Install Dependencies

Install the project dependencies from `pyproject.toml`:

```python
!pip install -e .
```

---

## 5. Configure Kaggle Credentials

Load your secrets and write the `kaggle.json` config file that the Kaggle CLI expects:

```python
import os
import json
from google.colab import userdata

os.makedirs(os.path.expanduser("~/.kaggle"), exist_ok=True)

kaggle_config = {
    "username": userdata.get("KAGGLE_USERNAME"),
    "key": userdata.get("KAGGLE_KEY"),
}

kaggle_json_path = os.path.expanduser("~/.kaggle/kaggle.json")
with open(kaggle_json_path, "w") as f:
    json.dump(kaggle_config, f)

os.chmod(kaggle_json_path, 0o600)

print("Kaggle credentials configured.")
```

---

## 6. Open and Run a Notebook

The notebooks are located in the `notebooks/` folder:

| Notebook | Description |
|---|---|
| `phase1_baseline_mlp.ipynb` | Baseline MLP classifier |
| `phase2_advanced_nn.ipynb` | Advanced neural network |
| `phase3_transfer_learning.ipynb` | Transfer learning |
| `phase4_optimization.ipynb` | Hyperparameter optimization |

To open a notebook from the file browser on the left, navigate to `notebooks/` and double-click the file, or use **File > Open notebook > Upload** to open one directly.

Run all cells with **Runtime > Run all**, or step through them with **Shift+Enter**.

---

## Notes

- Colab sessions disconnect after periods of inactivity. If your session resets, re-run steps 3–5 before continuing.
- The dataset will be downloaded to the path specified in `src/data_loader.py` on first run. If your session resets, the data is lost and will need to be re-downloaded unless you save it to Google Drive.
