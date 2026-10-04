# CHANCES Low-$Z$ Galaxy Cluster Membership with Random Forest

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)

Code and models accompanying the publication:
> **[Mapping cluster infall regions I:  
Identification of galaxies\\ in the neighborhood of clusters  out to  5R$_{200}$]**  
> *Author List*  
> *Journal / Year* — [DOI / Link]


We use a RandomForestClassiffier to tackle the membership task up to $5\,R_{vir}$


DESCRIPTION OF THE MODEL AND FUNCTIONALITY












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
3.  **Run the script:**

```bash
python main.py
```
or use the .ipynb instead

Then, to test, run:

```bash
python test.py
```


## Repository Structure 

```bash

TENTATIVE
├── data/
│   ├── clust_to_sim_all_dyn_state_v4_wo_repeated_update_relax_cv.fits        #>
│   └── cache/                      # Precomputed data files
│       └── df_rmag_20_4.parquet    # Pre-processed table for a 20.4 r-band cut
│       └── df_rmag_18_5.parquet    # Pre-processed table for a 18.5 r-band cut
├── models/
│   └── rf_reference_params.pkl     # Pre-trained Random Forest model for refer>
├── main.py                         # Execution script
├── main.ipynb                      # Notebook equivalent to execution script
├── test.py                         # Testing and validation script
├── plots.py                        # Plotting functions script
├── requirements.txt                # Required Python dependencies
└── README.md
```

---
