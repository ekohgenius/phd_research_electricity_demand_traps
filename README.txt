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

The analysis is calibrated primarily to the Rwanda Multi-Tier Framework (MTF) Energy Survey, 2022, and then applied comparatively to North-West Nigeria using the Nigeria Multi-Tier Framework (MTF) survey data.

The main analysis file is:

journal_paper.ipynb


Repository structure
====================

The repository is organized as follows:

.
├── README.txt
├── Dockerfile
├── requirements.txt
├── journal_paper.ipynb
├── raw_data/
│   └── README.txt
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
    └── additional diagnostic and robustness figures


Main files
==========

journal_paper.ipynb
-------------------

This is the main notebook used to construct the household agents, run the model, generate the manuscript figures, and export supplementary tables.

Dockerfile
----------

The Dockerfile defines a reproducible Python environment for executing the notebook.

requirements.txt
----------------

This file lists the Python dependencies used in the project.

created_csv_files/
------------------

This folder contains generated CSV and JSON outputs used for diagnostics, manuscript tables, and supplementary information.

figures/
--------

This folder contains generated figures from the notebook, including the manuscript-ready figures in PDF and PNG formats.

raw_data/
---------

This folder is reserved for input data. Raw survey microdata are not included in this public repository. See `raw_data/README.txt` for instructions on obtaining and placing the required Rwanda and Nigeria MTF files.


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

A minimal requirements file is provided as `requirements.txt`.


Running locally
===============

To run the analysis locally:

1. Install the dependencies:

   pip install -r requirements.txt

2. Download the required Rwanda and Nigeria MTF data following the instructions in:

   raw_data/README.txt

3. Place the required files in the expected folders inside `raw_data/`.

4. Open the notebook:

   jupyter notebook journal_paper.ipynb

5. Run all cells from top to bottom.

6. Generated figures and tables will be written to:

   figures/
   created_csv_files/


Running with Docker
===================

To build the Docker image:

docker build -t electrification-paper .

To execute the notebook:

docker run --rm -v "$PWD/created_csv_files:/app/created_csv_files" -v "$PWD/figures:/app/figures" electrification-paper

The Dockerfile is configured to execute:

journal_paper.ipynb

using Jupyter nbconvert.

If your notebook expects local data files, make sure the required data are present in `raw_data/` before running Docker.


Expected input data
===================

The analysis expects two main data sources:

1. Rwanda Multi-Tier Framework Energy Survey, 2022.
2. Nigeria Multi-Tier Framework survey data for the North-West Nigeria sample.

The Rwanda data are obtained from the World Bank Microdata Library and may require login. The Nigeria data are available through the World Bank Data Catalog and EnergyData.info. See `raw_data/README.txt` for details.


Expected outputs
================

The notebook produces the following types of outputs:

1. Household-agent construction files.
2. Model-input summary files.
3. Rwanda and Nigeria comparison tables.
4. Service-quality sensitivity grids.
5. Robustness grids and combined perturbation scenarios.
6. Manuscript-ready figures.
7. Supplementary figures and diagnostic plots.

Examples of generated output files include:

created_csv_files/rwanda_nigeria_comparison_table.csv
created_csv_files/supplement_S2_service_quality_summary.csv
created_csv_files/supplement_S6_robustness_full_grid_percent.csv
figures/fig1_rwanda_diagnostic.pdf
figures/fig2_rwanda_allocation.pdf
figures/fig3_state_dependence.pdf
figures/fig4_dynamics.pdf
figures/fig5_robustness.pdf


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

Raw survey microdata are not included in this public repository. Users should obtain the input data directly from the original providers and comply with the relevant access terms and licenses.

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

Email: [add preferred email address]
