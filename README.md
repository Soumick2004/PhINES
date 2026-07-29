# GNN-Based Human–Virus Interaction Prediction

This project trains a graph neural network (GCN) to predict interactions between human proteins and SARS-CoV-2 proteins using sequence-based embeddings from ProtBERT. The workflow combines:

- protein sequence embedding generation,
- graph construction from interaction pairs,
- GCN-based edge classification, and
- 5-fold cross-validation with standard classification metrics.

The repository is designed to run on Windows with a dedicated Python virtual environment named `gnn_env`.

---

## Project Overview

The main idea is to represent proteins as nodes in a graph and learn whether a human–virus pair is a true interaction or a negative example. The notebooks use:

- ProtBERT embeddings for protein sequences,
- PyTorch and PyTorch Geometric for graph modeling,
- scikit-learn for evaluation metrics,
- pandas and NumPy for data processing,
- matplotlib for visualization.

This is a research-style workflow and is intended for experimentation, model evaluation, and generating plots such as ROC/PR curves and loss trends.

---

## Repository Structure

- `InitialTraining.ipynb`  
  Main training notebook. Loads the positive and negative datasets, generates embeddings, trains the GCN model, and prints evaluation metrics.

- `Neg_Edge_SamplingTest.ipynb`  
  Notebook for generating a custom negative edge set and training the same model using that sampled negative pool.

- `SequenceIntegration.ipynb`  
  Supporting notebook for sequence/data preparation and integration tasks.

- `Data Files/`  
  Contains the datasets and sequence files used by the notebooks.

- `Total_Sampled_CustomNegative.csv`  
  Pre-generated custom sampled negative set that can be used directly by the second notebook.

- `gnn_env/`  
  Virtual environment created for this project. The environment is based on Python 3.11.9.

---

## Required Environment

### Python version

The environment in this project was created with Python 3.11.9.

### Operating system

This project was prepared for Windows.

### GPU requirement

A CUDA-capable NVIDIA GPU is strongly recommended for faster ProtBERT embedding and GCN training. The notebooks automatically detect GPU availability and use it when possible:

- if a GPU is available, training will run on CUDA,
- if not, the code will fall back to CPU.

CPU execution is possible, but it will be significantly slower for embedding generation and model training.

---

## Virtual Environment Setup

A virtual environment named `gnn_env` is already included in the project folder. If you want to recreate it manually, use the following commands in PowerShell:

```powershell
cd "C:\Users\soumi\OneDrive\Desktop\Innovative-Project"
python -m venv gnn_env
.\gnn_env\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

If you are using a GPU, install a CUDA-enabled PyTorch build that matches your system. Otherwise, a standard CPU build is enough.

### Core packages to install

Run the following inside the activated environment:

```powershell
pip install torch torch-geometric transformers scikit-learn pandas numpy matplotlib biopython jupyter
```

If `torch-geometric` installation gives version issues, install the correct PyTorch Geometric build for your installed PyTorch version from the official PyG installation guide.

---

## Important Data Files

### Required input files

The notebooks depend on the following data files:

- `Data Files/positive_ProjectFinal.csv`  
  Positive interaction pairs and associated sequences.

- `Data Files/negative_ProjectFinal_filtered_score0.1.csv`  
  Large negative interaction pool used for training. This file is required by the main training notebook.

- `Total_Sampled_CustomNegative.csv`  
  Custom sampled negative file generated from the large negative pool. This file is already included in the repository root and is used by the negative-sampling notebook.

### About the negative file

The large negative dataset was too large to store directly on GitHub. Because of that, it is not included in the repository and must be downloaded separately.

The notebook includes the download link:

- Google Drive link: https://drive.google.com/file/d/1Aaw5wcOaLjBBvab3sXhod25xtk9jmJUp/view?usp=drive_link

Once downloaded, place the file in the `Data Files/` folder with the expected name:

- `Data Files/negative_ProjectFinal_filtered_score0.1.csv`

If you do not want to download the full file manually, you can use the already available `Total_Sampled_CustomNegative.csv` for the custom negative sampling workflow.

---

## How to Run the Notebooks

### 1. Activate the environment

```powershell
cd "C:\Users\soumi\OneDrive\Desktop\Innovative-Project"
.\gnn_env\Scripts\Activate.ps1
```

### 2. Start Jupyter Notebook

```powershell
jupyter notebook
```

### 3. Open and run the notebooks

#### Option A: Run the main training notebook

Open `InitialTraining.ipynb` and run all cells in order.

This notebook will:

- load positive and negative data,
- embed proteins with ProtBERT,
- build the graph representation,
- train the GCN model using 5-fold cross-validation,
- generate ROC and PR plots,
- print the final evaluation metrics.

#### Option B: Run the custom negative generation workflow

Open `Neg_Edge_SamplingTest.ipynb` and run all cells in order.

This notebook will:

- load the positive and full negative data,
- remove overlaps with positive pairs,
- build a sampled negative subgraph using a 60/40 node split logic,
- save a new file named `Total_Sampled_CustomNegative.csv`,
- train the model using the custom sampled negatives.

If the notebook kernel does not use the virtual environment, select the Python interpreter from the `gnn_env` folder in the Jupyter kernel menu.

---

## Output and Evaluation Metrics

The notebooks report several useful metrics to evaluate the model.

### Metrics used

- Accuracy  
  Overall fraction of correct predictions.

- Precision  
  How many predicted positive interactions were actually correct.

- Recall  
  How many real positive interactions were correctly found.

- F1 Score  
  Harmonic mean of precision and recall.

- ROC-AUC  
  Measures how well the model ranks positives above negatives.

- AUPRC  
  Precision-recall area under the curve, useful for imbalanced datasets.

- Balanced Accuracy  
  Useful when classes are imbalanced.

- Matthews Correlation Coefficient (MCC)  
  A balanced classification metric that considers all confusion-matrix entries.

### Example results from the current workflow

The training notebook produced the following example metrics in the recorded run:

- Accuracy: 0.8234
- Precision: 0.8200
- Recall: 0.8916
- F1 Score: 0.8329
- AUC-ROC: 0.8753
- Balanced Accuracy: 0.8253

These values indicate a strong-performing model on the evaluated dataset, with especially good recall.

---

## Expected Outputs

Running the notebooks will generate:

- histogram plots of positive and negative score distributions,
- training loss logs across epochs,
- ROC curve plots,
- precision-recall curve plots,
- final metric summaries printed in the notebook output.

---

## Notes for Reproducibility

- Keep the notebook files and data folder in the same project structure as shown here.
- The notebooks use relative file paths, so running them from the project root is recommended.
- If you change the dataset location, update the file paths inside the notebooks.
- For best results, use the same Python version and package versions as the `gnn_env` environment.

---

## Quick Start Summary

1. Activate the environment.
2. Install the required packages.
3. Download `Data Files/negative_ProjectFinal_filtered_score0.1.csv` from the provided link if needed.
4. Run `Neg_Edge_SamplingTest.ipynb` to create the custom negative set.
5. Run `InitialTraining.ipynb` to train and evaluate the model.
