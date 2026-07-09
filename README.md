# BMI-ISRSA
A Python pipeline for Inter-Subject Representational Similarity Analysis (IS-RSA) of naturalistic fMRI, including sliding-window BOLD analysis, motion correction, and confound-controlled brain–phenotype association testing.


# Inter-Subject Representational Similarity Analysis (IS-RSA)

This repository implements an Inter-Subject Representational Similarity Analysis (IS-RSA) pipeline for investigating how individual differences in Body Mass Index (BMI) are reflected in shared brain responses during naturalistic movie watching.

The pipeline uses parcel-wise functional brain representations from the Human Connectome Project (HCP) 7T naturalistic viewing dataset to quantify inter-subject neural similarity and test its relationship with BMI similarity. Multiple IS-RSA models are implemented, including nearest-neighbor and Anna Karenina similarity models, with permutation-based statistical inference and false discovery rate (FDR) correction.

# Naturalistic fMRI ISRSA Pipeline

This repository provides a Python pipeline for performing Inter-Subject Representational Similarity Analysis (IS-RSA) on naturalistic fMRI data. The framework is designed to investigate relationships between inter-subject similarity in functional brain responses and phenotypic variables while accounting for potential confounding effects.

## Features

- Parcel-wise IS-RSA across the cortex
- Sliding-window BOLD signal analysis for dynamic brain representations
- Construction of inter-subject neural similarity matrices
- Brain–phenotype association testing using IS-RSA
- Motion confound correction using mean framewise displacement (RMSFD)
- IS-RSA with motion similarity matrices and movement regressors as control analyses
- Permutation-based significance testing
- False Discovery Rate (FDR) correction for multiple comparisons
- Modular pipeline for naturalistic fMRI datasets
