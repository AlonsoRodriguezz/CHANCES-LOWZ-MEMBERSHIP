# CHANCES Low - $z$ Galaxy Cluster Membership with Random Forest

![CHANCES Logo](https://chances.uda.cl/wp-content/uploads/2025/10/logo_chances_2022.png)

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)

This repository contains the machine learning pipeline developed for the publication:

> **Mapping cluster infall regions I:  
Identification of galaxies in the neighborhood of clusters  out to  $5 \mathrm{R}_{200}$**  
> *Franco Piraino-Cerda & Authors*  
> *Journal / Year* — [DOI / Link]

The code classiffies galaxy cluster members out to $5\ \mathrm{R}_{200}$ and is trained on mock catalogs from the CHANCES Low - $z$ sub-survey.

## Repository Structure 

```bash
├── data/
│   ├── clust_to_sim_all_dyn_state_relax.fits  # Cluster properties table
│   └── df_rmag_20_4.parquet                   # Precompued table for a 20.4 r-band cut
│   └── df_rmag_18_5.parquet                   # Precomputed table for a 18.5 r-band cut
│   └── rf_reference_params.pkl                # Pre-trained Random Forest model for reference parameters
├── GridsearchCV.py                            # GridsearchCV scheme as in the paper
├── main.py                                    # Execution script
├── main.ipynb                                 # Notebook equivalent to execution script
├── test.py                                    # Testing and validation script
├── plots.py                                   # Plotting functions script (imported by main)
├── requirements.txt                           # Python dependencies
└── README.md
```

## Methodology Summary

The pipeline implements the following workflow:

1. **Data Processing:** Reads mocks, links cluster properties from `clust_to_sim_all_dyn_state_relax.fits`, and loads or generates the corresponding `.parquet` table for a given $r$-band cut.

2. **Feature Selection and Engineering:**
   * r-band magnitude $m_r$
   * Log-local density estimation $\mathrm{log} \Sigma_{10}$
   * Phase-space parameters $R_{\mathrm{norm}} = \frac{r_{\mathrm{proj}}}{R_{200}}$, $V_{\mathrm{norm}}=\frac{V_{\mathrm{pec}}}{\sigma_{200}}$
   * Associated halo virial mass $M_{200}$

3. **Model Application:** Reads reference hyperparameters from `rf_reference_model.pkl` to train the Random Forest classifier, using a LeaveOneGroupOut cross-validation scheme.

4. **Output:** Exports membership predictions and generates diagnostic figures to a new directory.

## Installation & Usage

1.  **Clone the repository:**    
    ```bash
    git clone https://github.com/4MOST-CHANCES/CHANCES_LOWZ_MEMBERSHIP.git
    cd chances-lowz-membership
    ```
2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Configure Mock Catalog Paths:**
    The pipeline requires CHANCES mock catalogs to operate:
    * **`main.py` / `main.ipynb`:** requires your **training mocks**.
    * **`test.py` / `test.ipynb`:** requires the mocks allocated for **model validation/testing**.

    **You must ensure the mock directory path is correctly set:**

    In `main.py`/`main.ipynb` (and similarly in `test.py`/`test.ipynb`), check the directories block:

    ```python
    # Directories
    base_dir = os.path.dirname(os.path.abspath(__file__))

    # Option A: Place your mocks inside the repo folder (e.g. 'repo_folder/Mocks/')
    mocks_path = os.path.join(base_dir, 'Mocks')

    # Option B: Point to an external storage directory
    mocks_path = '/path/to/your/external/storage/Mocks/'
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

## Citation

If you use this pipeline or pre-trained models, please cite the corresponding paper:

(REPLACE WITH BIBTEX)
```bibtex
@article{...,
  author = {...},
  title  = {...},
  journal= {...},
  year   = {...}
}
```
