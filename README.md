# ML-Based Recommendation System for 4G RAN Optimization

## 1. Project Overview

This project implements an end-to-end machine learning pipeline to proactively identify and diagnose high-utilization sectors in a 4G LTE network. The system moves beyond simple prediction by providing specific, actionable recommendations for network engineers to perform traffic offloading and optimize network performance at the cell level.

The core of the system is a hybrid modeling approach that uses:
* **Hierarchical Graph Neural Network (H-GNN)** concepts to learn rich feature representations (embeddings) that capture the complex topological relationships between sectors and their underlying cells.
* An **XGBoost** model that leverages these embeddings along with traditional KPI and configuration features for highly accurate congestion prediction.
* **Explainable AI (XAI)** using the SHAP library to diagnose the root causes of predicted congestion for any given sector.
* A **Rule-Based Recommendation Engine** to translate the diagnosis into concrete actions for engineers to investigate.

The final output is a comprehensive, multi-sheet Excel report detailing problematic sectors, their offload candidates, and the underlying cell parameters.

---

## 2. Project Structure

The repository is organized into the following directories:

```
ML_based-Pro-LTE-Opt/
│
├── .gitignore          # Specifies files for Git to ignore (e.g., data, venv)
├── README.md           # This documentation file
├── requirements.txt    # Python library dependencies for reproducibility
│
├── Data/               # Contains all raw, intermediate, and final data files. (Ignored by Git)
│   └── final_report/   # Output directory for the final Excel reports.
│
├── docs/               # Directory for additional project documentation.
│
├── notebook/           # Contains Jupyter notebooks for exploration and visualization.
│
├── src/                # Contains all Python source code and scripts.
│   └── load_and_clean.py # Example script for initial data prep.
│   └── report_generator.py # The main script to run the full pipeline.
│
└── venv/               # Python virtual environment. (Ignored by Git)
```

---

## 3. Data & Feature Engineering Workflow

The project follows a systematic data preparation pipeline:

1.  **Data Ingestion:** Loads over 10 different CSV files containing sector-level KPIs, cell-level KPIs, detailed NBR studies, and various cell configuration parameters.
2.  **Identifier Standardization:** A crucial step where custom functions create `Canonical_Cell_ID` and `Canonical_Sector_ID` to ensure all data sources can be reliably linked.
3.  **Graph Component Creation:** The standardized data is transformed into five key components for the GNN model:
    * `graph_cell_node_features_V1.3.csv`
    * `graph_cell_edge_list_V1.3.csv`
    * `graph_sector_node_features_V1.3.csv`
    * `graph_sector_edge_list_V1.3.csv`
    * `graph_hierarchy_map_V1.3.csv`
4.  **Feature Engineering:** The final model uses features including static configurations, NBR suitability scores, and aggregated cell parameters (like average reference signal power and ET values).

---

## 4. Setup and Installation

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd ML_based-Pro-LTE-Opt
    ```
2.  **Create and activate a Python virtual environment:**
    * `python -m venv venv`
    * On Windows (Git Bash): `source venv/Scripts/activate`
    * On macOS/Linux: `source venv/bin/activate`
3.  **Install the required libraries:**
    ```bash
    pip install -r requirements.txt
    ```

---

## 5. How to Run

1.  Ensure all source `.csv` files are placed inside the `Data/` directory.
2.  Navigate to the `src/` directory from your terminal.
3.  Execute the main reporting script:
    ```bash
    cd src
    python report_generator.py  # Assuming you name the final script 'report_generator.py'
    ```
4.  The final output, `Full_Sector_Recommendation_Report_V2.2.xlsx`, will be generated in the `Data/final_report/` directory.

---

## 6. Key Technologies Used

* **Data Manipulation:** pandas, NumPy
* **Machine Learning:** scikit-learn, XGBoost
* **Graph Machine Learning:** PyTorch, PyTorch Geometric
* **Model Explainability:** SHAP
* **Graph Analysis & Visualization:** NetworkX, Matplotlib
* **Excel Reporting:** XlsxWriter
