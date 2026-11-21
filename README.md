# Fitness Tracker — Sensor Data & ML Pipeline

An end-to-end machine learning data preparation pipeline for wearable fitness sensor data (accelerometer and gyroscope streams), structured with clean separation between raw inputs, intermediate resampled datasets, and processed modeling features.

Read Raw CSVs — Accelerometer & Gyroscope files.



Extract Metadata — Participant, label, category, set.



Combine Files — All sensor CSVs concatenate kiye.



Datetime Conversion — Epoch timestamp ko DatetimeIndex banaya.



Merge Sensors — Accelerometer + Gyroscope.



Resampling — 200ms / 5Hz.



Export — data/interim/processed_data.pkl.

## Data Pipeline Overview

1. **Sensor Ingestion:** Loads raw wearable motion data containing time-indexed **3-axis Accelerometer** and **3-axis Gyroscope** signals (6 sensor channels total).
2. **Temporal Resampling:** Aligns irregular timestamps onto a uniform **5 Hz** grid (**200 ms** intervals), averaging readings within each window to maintain a consistent `DatetimeIndex`.
3. **Intermediate Export:** Writes the combined dataset to `data/interim/processed_data.pkl` using Pandas pickle serialization so `DatetimeIndex` and column `dtypes` are preserved for downstream steps.

## Setup & Usage

### 1. Create and Activate Virtual Environment (PowerShell)

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### 2. Run the Dataset Pipeline

```powershell
python -m src.data.make_dataset
```

Expected output artifact:
- `data/interim/processed_data.pkl`

### 3. Verify the Saved Dataset

```python
import pandas as pd

df = pd.read_pickle("data/interim/processed_data.pkl")
print(df.shape)
print(df.index.dtype)
print(df.head())
```

## Notes on Version Control

Large raw sensor files, `.venv/`, and serialized `.pkl` artifacts are excluded via `.gitignore` so the [Fitness Tracker repository](https://github.com/alokkumary0715-lgtm/Fitness_tracker) stays lightweight. Use local paths or external storage/DVC when sharing datasets across machines.