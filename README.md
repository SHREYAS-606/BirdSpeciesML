# Bird Species Identification from an Image

**Machine Learning Mini-Project — UE24CS352A**  
**Team Number:** 06  
**Project Number:** 89

**Team members**
- S Shreyas Kumar — PES2UG24CS656
- Raksha — PES2UG24CS652

## 1. Project Overview

This project explores multi-class bird-species classification using annotated visual attributes from the **CUB-200-2011 (Caltech–UCSD Birds)** dataset.

The current implementation uses structured attributes as model inputs; it does **not** classify raw image pixels or directly predict a species from a newly uploaded photograph.

The project has two goals:

1. Implement baseline classifiers and compare their performance with the results reported in the reference study, *“Bird Species Identification from an Image.”*
2. Extend the baseline work with **XGBoost**, evaluated on the same train/test split.

> **Status:** The baseline models and XGBoost have been evaluated in the notebook. Full reproduction of every experiment in the reference paper, including all PCA and feature-selection experiments, may still require additional work.

## 2. Dataset

The project uses CUB-200-2011, which contains:

- **11,788** bird images
- **200** species/classes
- **312** binary visual attributes per image
- **3,677,856** rows in the attribute annotation file used by this project

The notebook uses these dataset files:

- `attributes/image_attribute_labels.txt` — image ID, attribute ID, and presence flag
- `image_class_labels.txt` — mapping from image ID to species class ID

Download and extract the CUB-200-2011 dataset from its official source or the dataset link provided by your course/reference paper. Keep the dataset files in the expected directory structure, or update the dataset path in the notebook.

## 3. Methodology

1. Read the attribute annotation file.
2. Retain the image ID, attribute ID, and attribute-presence flag.
3. Pivot the annotation records into one row per image with 312 attribute columns.
4. Merge the attribute features with the species labels.
5. Separate the feature matrix (`X`) from the target labels (`y`).
6. Create a **stratified 70:30 train/test split** using `random_state=42`.
   - Training samples: **8,251**
   - Testing samples: **3,537**
7. Train and evaluate the baseline classifiers and the additional XGBoost model using test accuracy.

## 4. Models and Current Results

The following are the current notebook test accuracies. They are preliminary implementation results, not a claim that every experiment from the reference paper has been reproduced.

| Model | Test accuracy |
|---|---:|
| Bernoulli Naive Bayes | 45.60% |
| K-Nearest Neighbors (k = 5) | 33.50% |
| Decision Tree | 26.89% |
| Random Forest (100 trees) | 46.59% |
| Linear Discriminant Analysis (LDA) | 46.76% |
| **XGBoost (extension)** | **49.39%** |

### XGBoost configuration

The additional model was configured with:

```python
XGBClassifier(
    n_estimators=300,
    max_depth=10,
    learning_rate=0.1,
    subsample=0.8,
    colsample_bytree=0.8,
    random_state=42,
    eval_metric="mlogloss",
)
```

The notebook maps the 200 species labels to integer class indices `0` through `199` for XGBoost training. Its reported test accuracy is **0.493921**, or **49.39%**.

## 5. Requirements

- Python 3.10 or newer recommended
- CUB-200-2011 dataset files
- Jupyter Notebook or JupyterLab

Python packages used by the workflow:

- `numpy`
- `pandas`
- `scikit-learn`
- `xgboost`
- `matplotlib`
- `seaborn`
- `jupyter`

## 6. Setup and Run

### Windows (PowerShell)

1. Clone the repository and enter its folder:

   ```powershell
   git clone <YOUR-PRIVATE-REPOSITORY-URL>
   cd <YOUR-REPOSITORY-FOLDER>
   ```

2. Create and activate a virtual environment:

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

   If PowerShell blocks activation, you can run the environment's Python directly, or follow your institution's guidance for PowerShell execution policies.

3. Install dependencies:

   ```powershell
   python -m pip install --upgrade pip
   pip install numpy pandas scikit-learn xgboost matplotlib seaborn jupyter
   ```

4. Place the extracted CUB-200-2011 dataset in the location expected by the notebook. If your folder structure differs, edit the dataset path in the notebook before running it.

5. Start Jupyter:

   ```powershell
   jupyter notebook
   ```

6. Open the project notebook (`.ipynb`) and run its cells from top to bottom. Ensure the dataset path is correct before running the data-loading cells.

### Dataset path note

The notebook previously used a path similar to:

```python
"../data/CUB_200_2011/CUB_200_2011"
```

This path is relative to the notebook's working directory. Your local folder layout may differ, so verify or update the path as needed.

## 7. Repository Structure

A suggested repository structure is:

```text
bird-species-identification/
├── README.md
├── notebooks/
│   └── 01_dataset_check_2.ipynb
├── report/
│   └── Bird_Species_Identification_Two_Page_Writeup.pdf
└── data/
    └── CUB_200_2011/
        └── CUB_200_2011/
            ├── attributes/
            │   └── image_attribute_labels.txt
            └── image_class_labels.txt
```

Adapt the names to match the files you actually commit. Large dataset files may be better kept out of GitHub; if you do not include the dataset in the repository, document where to download it and where to place it locally.

## 8. Conclusion

The project builds an attribute-based pipeline for classifying 200 bird species. Among the current baseline implementations, LDA achieved **46.76%** test accuracy. XGBoost, added as the extension model, achieved **49.39%** on the same holdout split. These results provide an initial comparison; the final report should distinguish current notebook outputs from experiments that have been fully reproduced from the reference paper.

## References

1. Aditya Bhandari, Ameya Joshi, and Rohit Patki, *“Bird Species Identification from an Image”* (reference study).
2. CUB-200-2011 dataset, Caltech–UCSD Birds.
3. Scikit-learn documentation: https://scikit-learn.org/
4. XGBoost documentation: https://xgboost.readthedocs.io/
