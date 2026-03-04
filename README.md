# SMRAI_SD_SUBSTORM
# SMRAI with SuperDARN Data Assimilation for Substorm Analysis


This repository contains analysis workflows for comparing **SMRAI2.1** ionospheric electrodynamics with **SuperDARN data assimilation (DA)** during isolated substorm events. The notebooks use OMNI solar wind data, SMRAI2 machine-learning models, and SMRAI2.1 potential/electric field outputs to compute AL/AU indices and produce spatial visualizations.

---

## Overview

- **SMRAI2**: Provides electric potential and Hall conductivity via trained models driven by solar wind inputs (Refer Kataoka et al.,2024).
- **SMRAI2.1**: Potential/electric field files are used for auroral current calculations (Refere Nakano et al., 2025).
- **Analysis**: AL/AU index time series and multi-panel polar plots for selected substorm intervals (e.g., 2023-10-30, 2023-10-31).

---

## Notebooks

### 1. `predict_al_au_figure2.ipynb`

**Purpose:** Compute and compare AL/AU indices from SMRAI2.1 control and SuperDARN DA runs, and produce time series comparison plots (Figure 2–style output).

**Workflow:**

- **OMNI solar wind data**
  - Downloads OMNI 5-minute data for a user-specified time range.
  - Default dates: **2023-10-30** or **2023-10-31** (configurable in the notebook).

- **SMRAI2 models**
  - Uses `modelp099_250.sav` and `modelw099_300.sav` to predict:
    - Electric potential
    - Hall conductivity
  - Driven by solar wind inputs 

- **SMRAI2.1 electric field inputs**
  - Loads electric field files for:
    - SMRAI2.1 **control** (no data assimilation)
    - SMRAI2.1 **with SuperDARN DA** (SMRAI2.1_DAssim)

- **Grid and conductivity**
  - Interpolates conductivity fields from the SMRAI2 grid onto the SMRAI2.1 spatial grid for consistent comparison.

- **AL/AU indices**
  - Computes AL and AU from derived Hall currents in the **auroral zone** (60–70° latitude band).
  - Produces two AL/AU series:
    - **SMRAI2.1 control** (without data assimilation)
    - **SMRAI2.1 with SuperDARN data assimilation** (SMRAI2.1_DAssim)

- **Output**
  - Time series comparison plots showing the performance of each approach over the selected interval.

---

### 2. `four_para_plot_figure4.ipynb`

**Purpose:** Generate detailed **spatial** visualizations at selected times for model outputs and SuperDARN coverage (Figure 4–style multi-panel plots).

**Workflow:**

- **Time and date**
  - You can change the **time** and **date** in the notebook to focus on specific substorm phases.

- **Multi-panel polar plots**
  - **Ionospheric potential fields** for:
    - SMRAI2.1
    - SMRAI2.1 with SuperDARN DA
  - **Conductivity distributions** 
  - **SuperDARN observational coverage** (where DA observations are available)

- **Use case**
  - Visual inspection of model outputs and their spatial relationships during chosen substorm phases.

---

## Data and Model Dependencies

| Item | Description |
|------|-------------|
| **OMNI data** | Fetched via `pyspedas` (5-minute resolution) for the chosen date range. |
| **SMRAI2 models** | `SMRAI2/modelp099_250.sav`, `SMRAI2/modelw099_300.sav`; plus `meanp.npy`, `imgp.npy`, `meanw.npy`, `imgw.npy` for potential and conductivity. |
| **SMRAI2.1 outputs** | Electric potential and electric field (and related) files for control and SuperDARN DA runs (e.g., under `SMRAI2.1/substorm_20231030/`). |

---
## Requirements

Typical dependencies (install as needed for your environment):
- Custom modules such as `esn_dts_openloop` (Refer Kataoka et al, 2024)
- `numpy`, `matplotlib`, `pandas`
- `pyspedas`, `pytplot` (OMNI and time series)
- `geopack` (e.g., for coordinate/IMF handling)
- `scikit-learn` (e.g., for PCA/ML components in SMRAI2)


## Usage Summary

1. **AL/AU and time series (Figure 2)**  
   Run `predict_al_au_figure2.ipynb`. Set the date range (e.g., 20231030 or 20231031) in the first cells; the notebook will download OMNI data, run SMRAI2, load SMRAI2.1 fields, interpolate conductivity, compute AL/AU, and plot comparisons.

2. **Spatial snapshots (Figure 4)**  
   Run `all_para_plot_figure4.ipynb`. Set the date and time of interest, then execute to generate multi-panel polar plots of potentials, conductivities, and SuperDARN data coverage.

---


