# ISRSA Pipeline for Naturalistic fMRI

A Python pipeline for **Inter-Subject Representational Similarity Analysis (IS-RSA)** and complementary analyses of naturalistic fMRI data. The project was designed to investigate BMI-related brain representations during movie watching while assessing motion and demographic confounds.

## What this repository does

This repository supports parcel-wise IS-RSA workflows for HCP-style naturalistic fMRI data stored as a NumPy array with shape:

```text
subjects × TRs × parcels
```

The main analysis relates inter-subject neural similarity to BMI similarity. Additional scripts test whether results are robust to RMSFD and demographic covariates, whether BMI similarity is associated with motion similarity, and which time windows show high-vs-low BMI BOLD differences.

## Main features

- Primary parcel-wise IS-RSA using a rank-based BMI nearest-neighbor similarity model
- Exploratory BMI Anna Karenina similarity model
- Covariate-controlled IS-RSA using partial Spearman correlation
- RMSFD similarity analysis as a motion confound check
- Movement-regressor IS-RSA for translation and rotation parameters
- Sliding-window BOLD analysis comparing high- and low-BMI groups
- Parcel-wise ISC summaries
- Glasser/HCP network annotation
- Interactive cortical surface visualization with `hcp_utils` and `nilearn`

## Repository structure

```text
ISRSA-Pipeline/
├── scripts/
│   ├── run_primary_nn.py
│   ├── run_exploratory_annak.py
│   ├── run_covariate_controlled.py
│   ├── run_rmsfd_isrsa.py
│   ├── run_motion_regressor_isrsa.py
│   ├── run_secondary_isc.py
│   ├── sliding_window_bold.py
│   ├── annotate_networks.py
│   └── visualize_surface_hcp.py
├── src/isrsa/
│   ├── utils.py
│   ├── statistics.py
│   ├── covariates.py
│   └── plotting.py
├── data/
│   └── README.md
├── examples/
│   └── example_commands.sh
├── requirements.txt
├── pyproject.toml
├── .gitignore
└── LICENSE
```

## Installation

```bash
# Clone your repository, then from the repository root:
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install -e .
```

Optional surface visualization requires `hcp-utils` and `nilearn`.

## Required input files

### Brain data

A `.npy` file with shape:

```text
n_subjects × n_TRs × n_parcels
```

The scripts assume subjects are in the same order as the phenotype CSV.

### Phenotype CSV

Must include:

```text
Subject,BMI
```

Optional demographic covariates for covariate-controlled IS-RSA:

```text
Age, Sex/Gender, Race, Ethnicity
```

### RMSFD CSV

For motion/covariate analyses:

```text
Subject,RMSFD
```

### Movement regressors

For `run_motion_regressor_isrsa.py`, use a directory containing files named like:

```text
<Subject>_Movement_Regressors.txt
```

The first six columns are treated as:

```text
trans_x, trans_y, trans_z, rot_x, rot_y, rot_z
```

## Example usage

### Primary BMI nearest-neighbor IS-RSA

```bash
python scripts/run_primary_nn.py \
  --brain data/df_X_movie4_subjects_TRs_parcels.npy \
  --phenotype data/Subjects_Info.csv \
  --out-dir results/primary_nn_movie4 \
  --stimulus movie4 \
  --n-permute 5000 \
  --n-jobs 8
```

### Exploratory Anna Karenina IS-RSA

```bash
python scripts/run_exploratory_annak.py \
  --brain data/df_X_movie4_subjects_TRs_parcels.npy \
  --phenotype data/Subjects_Info.csv \
  --out-dir results/exploratory_annak_movie4 \
  --stimulus movie4
```

### Covariate-controlled IS-RSA

```bash
python scripts/run_covariate_controlled.py \
  --brain data/df_X_movie4_subjects_TRs_parcels.npy \
  --phenotype data/Subjects_Info.csv \
  --rmsfd data/RMSFD_subjects.csv \
  --out-dir results/covariate_controlled_movie4 \
  --stimulus movie4
```

### RMSFD confound check

```bash
python scripts/run_rmsfd_isrsa.py \
  --phenotype data/Subjects_Info.csv \
  --rmsfd data/RMSFD_subjects.csv \
  --out-dir results/rmsfd_movie4 \
  --stimulus movie4
```

### Movement-regressor IS-RSA

```bash
python scripts/run_motion_regressor_isrsa.py \
  --motion-dir data/movement_regressors/movie4 \
  --phenotype data/Subjects_Info.csv \
  --out-dir results/motion_movie4 \
  --stimulus movie4
```

### Sliding-window BOLD analysis

```bash
python scripts/sliding_window_bold.py \
  --brain data/df_X_bridgeville_subjects_TRs_parcels.npy \
  --phenotype data/Subjects_Info.csv \
  --out-dir results/bridgeville_sliding_window \
  --stimulus bridgeville \
  --parcels 336,157,3,2,186,6,227 \
  --window 20 \
  --step 1
```

## Outputs

This repository does **not** include figures, results, or example datasets. These files are generated locally when you run the scripts. Typical generated outputs include:

- Parcel-wise IS-RSA result CSVs
- Permutation p-values
- FDR q-values and significance indicators
- Included-subject CSVs
- BMI similarity matrix plots
- Parcel brain similarity sanity-check plots
- Sliding-window BOLD result and peak CSVs
- Network-annotated parcel result files
- Optional interactive HTML surface maps

## Notes

This repository does not include restricted HCP data, example data, generated results, or figures. Place data locally under `data/` or provide absolute paths when running scripts. Output directories such as `results/` and `figures/` will be created locally as needed and should generally not be committed.

## License

Apache License
Version 2.0, January 2004
