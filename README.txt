README.txt

Project title
=============

Hardening Grids or Financing Demand? State-Dependent Electrification Strategies in Africa


Project description
===================

This repository contains the reproducibility materials for the manuscript:

"Hardening Grids or Financing Demand? State-Dependent Electrification Strategies in Africa"

The study develops an agent-based model of post-electrification electricity use and utility investment decisions. The model compares two intervention strategies:

1. Grid hardening, which improves electricity service usability.
2. Appliance financing, which relaxes liquidity constraints and supports movement into higher electricity-use capacity tiers.

The analysis is calibrated primarily to the Rwanda Multi-Tier Framework (MTF) Energy Survey, 2022, and then applied comparatively to North-West Nigeria using Nigeria Multi-Tier Framework survey data.

The main analysis file is:

code/journal_paper.ipynb


Repository structure
====================

The repository is organized as follows:

.
├── README.txt
├── code/
│   └── journal_paper.ipynb
├── data/
│   ├── README.md
│   ├── ng_weights.dta
│   ├── raw-data_nigeria (2).zip
│   ├── NGA_2018_MTF_v01_M_CSV.zip
│   ├── NGA_2018_MTF_v01_M_CSV/
│   │   ├── MTF_NG_HH_Identification.dta
│   │   ├── MTF_NG_HH_SEC_B.dta
│   │   ├── MTF_NG_HH_SEC_C.dta
│   │   ├── MTF_NG_HH_SEC_K.dta
│   │   ├── MTF_NG_HH_SEC_N_ELEC_ASSET.dta
│   │   └── [other Nigeria MTF files]
│   └── RWA_2022_MTF_v01_M_CSV/
│       ├── household_survey_data/
│       │   ├── ROSTER_A1.csv
│       │   ├── SECTION_B.csv
│       │   ├── SECTION_C1.csv
│       │   ├── SECTION_DE.csv
│       │   ├── SECTION_H.csv
│       │   └── [other Rwanda MTF household files]
│       └── community_and_public_institutions_surveys_data/
│           └── [Rwanda community/public institution files]
├── environment/
│   ├── Dockerfile
│   └── requirements.txt
└── results/
    ├── created_csv_files/
    │   ├── abm_agents.csv
    │   ├── abm_inputs.csv
    │   ├── mtf_assumptions.json
    │   ├── rwanda_nigeria_comparison_table.csv
    │   ├── sector_aggregates.csv
    │   ├── supplement_S2_service_quality_grid_percent.csv
    │   ├── supplement_S2_service_quality_grid_raw.csv
    │   ├── supplement_S2_service_quality_summary.csv
    │   ├── supplement_S6_combined_scenarios_percent.csv
    │   ├── supplement_S6_combined_scenarios_raw.csv
    │   ├── supplement_S6_robustness_full_grid_percent.csv
    │   ├── supplement_S6_robustness_full_grid_raw.csv
    │   └── utility_baseline_2024.csv
    └── figures/
        ├── fig1_rwanda_diagnostic.pdf
        ├── fig1_rwanda_diagnostic.png
        ├── fig2_rwanda_allocation.pdf
        ├── fig2_rwanda_allocation.png
        ├── fig3_state_dependence.pdf
        ├── fig3_state_dependence.png
        ├── fig4_dynamics.pdf
        ├── fig4_dynamics.png
        ├── fig5_robustness.pdf
        ├── fig5_robustness.png
        └── [additional diagnostic and robustness figures]


Main files
==========

code/journal_paper.ipynb
------------------------

This is the main notebook used to construct household agents, run the model, generate the manuscript figures, and export supplementary tables.

environment/Dockerfile
----------------------

This Dockerfile defines the reproducible execution environment.

environment/requirements.txt
----------------------------

This file lists the Python packages required to run the notebook.

data/
-----

This folder contains or points to the raw input data required for the analysis. For public repositories, raw microdata should be excluded unless redistribution is clearly permitted by the relevant data license. See `data/README.md` for data access instructions.

results/created_csv_files/
--------------------------

This folder contains generated CSV and JSON outputs used for diagnostics, manuscript tables, and supplementary information.

results/figures/
----------------

This folder contains generated figures, including manuscript-ready PDF and PNG files.


Software requirements
=====================

The analysis uses Python with the following main packages:

- numpy
- pandas
- matplotlib
- seaborn
- jupyter
- nbconvert
- ipykernel

The package list is provided in:

environment/requirements.txt


Running locally
===============

To run the analysis locally from the repository root:

1. Install dependencies:

   pip install -r environment/requirements.txt

2. Confirm that the required input data are available in:

   data/

3. Open the notebook:

   jupyter notebook code/journal_paper.ipynb

4. Run all cells from top to bottom.

5. Generated figures and tables should be written to:

   results/figures/
   results/created_csv_files/


Running from the command line
=============================

From the repository root, execute:

jupyter nbconvert --to notebook --execute code/journal_paper.ipynb --output journal_paper_executed.ipynb

Depending on the notebook’s internal paths, the executed notebook may be written to the current working directory or to the `code/` folder. If needed, update the notebook output paths before execution.


Running with Docker
===================

The Dockerfile is located in:

environment/Dockerfile

To build the Docker image from the repository root:

docker build -f environment/Dockerfile -t electrification-paper .

To run the notebook from the Docker container:

docker run --rm \
  -v "$PWD/data:/app/data" \
  -v "$PWD/results:/app/results" \
  electrification-paper

If your Dockerfile uses a different working directory or command, update the paths accordingly.


Expected input data
===================

The analysis expects two main data sources:

1. Rwanda Multi-Tier Framework Energy Survey, 2022.
2. Nigeria Multi-Tier Framework survey data for the North-West Nigeria sample.

The Rwanda data are obtained from the World Bank Microdata Library and may require login. The Nigeria data are available through the World Bank Data Catalog and EnergyData.info. The Nigeria household weights file used in the analysis is referenced as:

data/ng_weights.dta

See `data/README.md` for full details.


Expected outputs
================

The notebook produces:

1. Household-agent construction files.
2. Model-input summary files.
3. Rwanda and Nigeria comparison tables.
4. Service-quality sensitivity grids.
5. Robustness grids and combined perturbation scenarios.
6. Manuscript-ready figures.
7. Supplementary figures and diagnostic plots.

Examples of generated output files include:

results/created_csv_files/rwanda_nigeria_comparison_table.csv
results/created_csv_files/supplement_S2_service_quality_summary.csv
results/created_csv_files/supplement_S6_robustness_full_grid_percent.csv
results/figures/fig1_rwanda_diagnostic.pdf
results/figures/fig2_rwanda_allocation.pdf
results/figures/fig3_state_dependence.pdf
results/figures/fig4_dynamics.pdf
results/figures/fig5_robustness.pdf


Model overview
==============

The model constructs connected-household agents from MTF microdata and evaluates two investment strategies.

Strategy A: Grid hardening
--------------------------

Grid hardening improves the household service-quality index while holding appliance capacity fixed.

Strategy B: Appliance financing
-------------------------------

Appliance financing relaxes liquidity constraints and allows households to move up by one capacity tier, subject to an adoption probability that depends on baseline reliability.

The model ranks interventions using the marginal efficiency of capital:

MEC_i,S = Delta R_i,S / C_S

where Delta R_i,S is the expected annual revenue gain from strategy S for household i, and C_S is the per-household intervention cost.


Service-quality index
=====================

Electricity service quality is represented using an effective usability index:

m(R_i) = alpha * s_day(H_day,i) + (1 - alpha) * s_eve(H_eve,i)

where:

- H_day,i is reported daily electricity availability.
- H_eve,i is reported evening electricity availability.
- s_day(.) is a concave transformation of daily availability.
- s_eve(.) is a capped transformation of evening availability.
- alpha controls the weight placed on daily versus evening availability.

Baseline parameters:

alpha = 0.7
beta = 0.15
gamma = 0.8

The daytime component is:

s_day(H_day,i) = [beta + (H_day,i / 24)^gamma] / (beta + 1)

The evening component is:

s_eve(H_eve,i) = min(1, H_eve,i / 4)


Capacity-tier assignment
========================

Connected households are assigned capacity tiers based on observed appliance ownership.

Tier 1:
Lighting and phone charging.

Tier 2:
Television, radio, fan, computer, DVD/VCD player, and modem/router.

Tier 3:
Refrigerator, freezer, rice cooker, and food processor/blender.

Tier 4:
Washing machine, electric iron, space heater, electric mill, water pump, sewing machine, and hair dryer.

Tier 5:
Air conditioner, electric water heater, and microwave oven.

Tier 0:
No grid or mini-grid connection. Tier 0 households are excluded from the connected-household strategy comparison.


Data-use note
=============

Raw survey microdata should not be redistributed in a public repository unless redistribution is clearly permitted by the relevant data license.

The Rwanda MTF 2022 microdata are accessed through the World Bank Microdata Library and may require login. The Nigeria MTF data are available through the World Bank Data Catalog and EnergyData.info.

This repository does not imply endorsement by the World Bank, EnergyData.info, or any other data provider. The original data collectors, authorized distributors, and funding agencies bear no responsibility for the analysis, interpretations, or conclusions presented in this project.


Code availability
=================

The code central to the simulations, robustness checks, and figure generation is organized in this repository.

If the manuscript proceeds to peer review, the code may also be shared through a Code Ocean capsule. The capsule/repository will include scripts, documentation, and instructions for obtaining the required input data and reproducing the analysis.


Suggested citation
==================

If using this repository, please cite the associated manuscript:

Ekoh, C., Batabyal, A. A., and Williams, N. J. Hardening Grids or Financing Demand? State-Dependent Electrification Strategies in Africa.

Please also cite the original data sources used in any reproduction or extension of this analysis.


Contact
=======

Courage Ekoh
Golisano Institute for Sustainability
Rochester Institute of Technology

Email: ce8760@rit.edu