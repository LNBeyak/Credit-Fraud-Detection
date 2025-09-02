# Credit-Fraud-Detection
##Project Summary: Reduce financial loss and investigation workload by detecting fraudulent transactions in near-real time using statistics, Python, R, ML, UX/survey, and SQL in a reproducable notebook.

##PROBLEM STATEMENT
Financial institutions face losses from fraudulent transactions and customer churn due to false positives. This project builds an imbalanced-classification system to predict whether a transaction is fraudulent at authorization time or not.

Primary objective:
Maximize fraud dollars blocked while minimizing customer impact (false positives), subject to latency constraints.

Constraints & considerations:
Highly imbalanced labels are fraud ≪ non-fraud

Latency: end-to-end scoring ≤ 150 ms p95 (local dev target)
Fairness & bias: monitor disparate impact by geography/merchant category; no protected attributes used directly
Concept drift: track model/feature drift monthly

##SUCCESS METRICS
Both Model and Business will be reported.

Model Metrics:
PR AUC (primary, better for imbalance than ROC AUC)
ROC AUC (secondary, for comparability)
Recall@K where K = top N% of transactions flagged
Precision@K at same operating points
FNR at fixed FPR (e.g., FPR = 0.5%)
KS statistic (optional, risk teams often use)

Cost-/Impact-Aware Metrics:
$ Fraud prevented = Σ(amount_i) for correctly blocked frauds
$ False-positive cost = (# false positives) × (avg operational cost per review/decline)
Net benefit = Fraud prevented − False-positive cost
Customer friction rate = (# non-fraud flagged) / (# non-fraud total)

Operational Metrics:
p95 scoring latency (ms)
Throughput (txn/sec in batch tests)
Drift scores (PSI for key features, monthly)

##REPOSITORY STRUCTURE
credit-fraud-detection/
├─ README.md
├─ LICENSE
├─ .gitignore
├─ pyproject.toml              # or requirements.txt
├─ .env.example
├─ data/
│  ├─ raw/                     # never commit PII; use git-crypt or keep local
│  ├─ interim/
│  └─ processed/
├─ notebooks/
│  ├─ 01_eda.ipynb
│  └─ 02_baseline_model.ipynb
├─ src/
│  ├─ __init__.py
│  ├─ data/                    # loaders, validators
│  │  ├─ load.py
│  │  └─ schema.py
│  ├─ features/                # feature builders
│  │  └─ build_features.py
│  ├─ models/                  # train/infer, thresholding
│  │  ├─ train.py
│  │  └─ predict.py
│  ├─ evaluation/
│  │  └─ metrics.py
│  └─ utils/
│     └─ logger.py
├─ scripts/
│  ├─ init_db.py               # create tables
│  ├─ ingest_sample.py         # optional: load demo CSV
│  └─ evaluate_baseline.py
└─ sql/
   ├─ create_tables.sql
   └─ indices.sql
