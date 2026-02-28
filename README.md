# DVC MLOps Lab 3 — Data Version Control with Google Cloud Storage

## Overview

This project demonstrates **Data Version Control (DVC)** integrated with **Google Cloud Storage (GCS)** for versioning datasets in a machine learning workflow. It covers initializing DVC, tracking datasets, pushing data to a remote GCS bucket, and reverting to previous dataset versions using Git and DVC together.

## Project Structure

```
dvc-mlops-lab3/
├── .dvc/                  # DVC configuration and cache
│   └── config             # Remote storage configuration
├── data/
│   ├── CC_GENERAL.csv.dvc # DVC tracking file for the dataset
│   └── .gitignore         # Ensures raw data isn't tracked by Git
├── .dvcignore             # Files/directories for DVC to ignore
├── requirements.txt       # Python dependencies
└── README.md
```

## Dataset

- **Source:** [Credit Card Dataset for Clustering](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata) (Kaggle)
- **File:** `CC_GENERAL.csv` — contains credit card holder behavior data with 18 features summarizing usage patterns for ~9,000 customers.

## Tech Stack

- **Python 3.10+**
- **DVC** — Data versioning and remote storage management
- **Google Cloud Storage (GCS)** — Remote storage backend for DVC
- **Git** — Source code and DVC metadata version control

## Setup & Installation

### 1. Clone the Repository

```bash
git clone https://github.com/jogeash/dvc-mlops-lab3.git
cd dvc-mlops-lab3
```

### 2. Create and Activate Virtual Environment

```bash
python -m venv venv
source venv/bin/activate        # macOS/Linux
# venv\Scripts\activate         # Windows
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure GCP Credentials

Place your GCP service account JSON key file in the project directory and configure DVC:

```bash
dvc remote modify myremote credentialpath <path-to-your-credentials.json>
```

### 5. Pull Data from Remote

```bash
dvc pull
```

## DVC Workflow

### Initialize DVC

```bash
dvc init
```

### Add Remote Storage

```bash
dvc remote add -d myremote gs://<your-bucket-name>
```

### Track a Dataset

```bash
dvc add data/CC_GENERAL.csv
git add data/CC_GENERAL.csv.dvc data/.gitignore
git commit -m "Track dataset with DVC"
```

### Push Data to GCS

```bash
dvc push
```

### Update Dataset Version

```bash
# Replace data/CC_GENERAL.csv with updated file
dvc add data/CC_GENERAL.csv
git add data/CC_GENERAL.csv.dvc
git commit -m "Updated dataset version"
dvc push
```

### Revert to a Previous Version

```bash
git checkout <commit-hash> -- data/CC_GENERAL.csv.dvc
dvc checkout
```

