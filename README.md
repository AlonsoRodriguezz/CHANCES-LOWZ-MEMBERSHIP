# CHANCES Low - $z$ Galaxy Cluster Membership with Random Forest

<img src="assets/logo_chances_2022.png" alt="CHANCES Logo" height="80">

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)

This repository contains the machine learning pipeline developed for the publication:

> **Mapping cluster infall regions I: Identification of galaxies in the neighborhood of clusters out to  $5 \mathrm{R}_{200}$**  
> *Franco Piraino-Cerda et al.*  
> *Journal / Year* — [DOI / Link]

The code classifies galaxy cluster members out to $5\ \mathrm{R}_{200}$ and is trained on mock catalogs from the CHANCES Low - $z$ sub-survey. For any inquiries please contact [Gerardo Rodríguez](https://github.com/AlonsoRodriguezz).

## Repository Structure 

```text
├── data/
│   ├── train/
│   │   ├── clust_to_sim_all_dyn_state_relax_train.fits  # Training cluster properties table
│   │   └── RF_clone.pkl                                 # Pre-trained Random Forest model for reference parameters
│   └── test/
│       └── clust_to_sim_all_dyn_state_relax_test.fits   # Validation cluster properties table
├── GridsearchCV_pipe.ipynb                              # GridsearchCV scheme as in the paper
├── main.py                                              # Training script
├── main.ipynb                                           # Notebook equivalent to training script
├── test.py                                              # Model testing script
├── test.ipynb                                           # Notebook equivalent to model testing script
├── plots.py                                             # Plotting functions script (imported by main)
├── requirements.txt                                     # Python dependencies
└── README.md
```

## Methodology Summary

The pipeline implements the following workflow:

1. **Data Processing:** Reads mocks, links cluster properties from `clust_to_sim_all_dyn_state_relax_train.fits`, and loads or generates the corresponding `*_master.parquet` table for a given $r$-band cut. If you want to save some time *you can download* the `df_rmag_20_4_master.parquet` and `df_rmag_18_5_master.parquet` (for a 20.4 and 18.5 $r$-band cut respectively) files from [this link (in the future)](link-to-drive) and store them in your data/train/ folder.

2. **Feature Selection and Engineering:**
   * r-band magnitude $m_r$
   * Log-local density estimation $\log\ \Sigma_{10}$
   * Phase-space parameters $R_{\mathrm{norm}} = \frac{r_{\mathrm{proj}}}{R_{200}}$, $V_{\mathrm{norm}}=\frac{V_{\mathrm{pec}}}{\sigma_{200}}$
   * Associated halo virial mass $M_{200}$

3. **Model Application:** Reads reference hyperparameters from `RF_clone.pkl` to train the Random Forest classifier, using a LeaveOneGroupOut cross-validation scheme.

4. **Outputs:** Generates an output directory containing:
   * `.pkl` **file**: Trained Random Forest model.
   * `.parquet` **file**: Contains predicted probabilities and membership classifications.
   * **Diagnostic figures:** Performance and validation plots (confusion matrix, correlation matrix, phase space, purity and completeness, feature importances, ROC curve, optional learning curve).

## Installation & Usage

1.  **Clone the repository:**    
    ```bash
    git clone https://github.com/AlonsoRodriguezz/CHANCES-LOWZ-MEMBERSHIP.git
    cd CHANCES-LOWZ-MEMBERSHIP
    ```
2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    > **Note for Anaconda users:** To avoid breaking your `base` environment due to version conflicts, we highly recommend creating a dedicated environment before installing the requirements:
    > ```bash
    > conda create -n chances_lowz python=3.10
    > conda activate chances_lowz
    > pip install -r requirements.txt
    > ```
    > Then, you can access your environment using:
    > ```bash
    > conda activate chances_lowz
    > ```
    > Later on, to deactivate the environment when you are done:
    > ```bash
    > conda deactivate
    > ```     
    
3.  **Configure Mock Catalog Paths:**
    The pipeline requires CHANCES mock catalogs to operate:
    * **`main.py` / `main.ipynb`:** requires your **training mocks**.
    * **`test.py` / `test.ipynb`:** requires your **model testing mocks**.

    **You must ensure the mock directory path is correctly set:**

    In `main.py`/`main.ipynb` (and similarly in `test.py`/`test.ipynb`), check the directories block:

    ```python
    # Directories
    base_dir = os.path.dirname(os.path.abspath(__file__))

    # Option A: Place your mocks inside the repo folder (e.g. 'repo_folder/Mocks_training/')
    mocks_path = os.path.join(base_dir, 'Mocks_training') # or 'Mocks_testing'

    # Option B: Point to an external storage directory
    mocks_path = '/path/to/your/external/storage/Mocks_training/'
    ```
    
4.  **Run the script over your training set**, you can run the full pipeline either from the terminal or interactively in a jupyter notebook:
    ```bash
    python main.py
    ```
    or use the `main.ipynb` instead.

5.  **Run the testing script over your test set:**
    ```bash
    python test.py
    ```
    or use the `test.ipynb` instead.
    > **Note:** If you only want to validate using a reference model (e.g. the one provided here), you can run `test.py`/`test.ipynb` directly and skip the training phase (`main.py`/`main.ipynb`), same thing applies for future runs once you have trained your own model running `main.py`. Always ensure the script points to your intended trained model path.

## Citation

If you use this pipeline or pre-trained models for your investigation, please cite the corresponding paper:

```bibtex
@article{...,
  author = {...},
  title  = {...},
  journal= {...},
  year   = {...}
}
```
