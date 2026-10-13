Project title
=============

Optimal electrification strategies in Africa depend on baseline electricity service quality


Project description
===================

This repository contains the code and source data for the manuscript "Optimal electrification strategies in Africa depend on baseline electricity service quality" by Courage Ekoh, Amitrajeet A. Batabyal and Nathaniel J. Williams.

The study develops an agent-based model of post-electrification electricity use and utility investment decisions. The model compares two intervention strategies:

1. Grid hardening, which improves electricity service usability.
2. Appliance financing, which relaxes liquidity constraints and supports movement into higher electricity-use capacity tiers.

The analysis is calibrated primarily to the Rwanda Multi-Tier Framework (MTF) Energy Survey, 2022, and then applied comparatively to North-West Nigeria using Nigeria Multi-Tier Framework survey data.

The main analysis file is:

code/journal_paper.ipynb


Repository structure
====================

.
├── README.txt
├── LICENSE
├── code/
│   └── journal_paper.ipynb
├── data/
│   └── README.md          (instructions for obtaining the survey microdata; raw data are not included)
├── environment/
│   ├── Dockerfile
│   └── requirements.txt
└── results/
    ├── source_data/        (per-figure source data; see below)
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
        ├── fig1_rwanda_diagnostic.pdf / .png
        ├── fig2_rwanda_allocation.pdf / .png
        ├── fig3_state_dependence.pdf / .png
        ├── fig4_dynamics.pdf / .png
        ├── fig5_robustness.pdf / .png
        └── [additional diagnostic and robustness figures]


Source data for main figures
============================

The numerical data underlying each main-text figure and Table 1 are in results/source_data/ (written by the final cell of code/journal_paper.ipynb). Only aggregated or model-derived values are included; no survey microdata.

Fig. 1a  fig1a_rwanda_usability_distribution.csv      (weighted histogram, 30 bins)
Fig. 1b  fig1b_rwanda_tier_by_location.csv
Fig. 1c  fig1c_rwanda_credit_ladder.csv
Fig. 2a  fig2a_rwanda_hardening_dominance_heatmap.csv
Fig. 2b  fig2b_rwanda_allocation_by_tier.csv
Fig. 2c  fig2c_rwanda_marginal_revenue_frontier.csv
Fig. 3a  fig3a_rwanda_usability_distribution.csv
Fig. 3b  fig3b_nigeria_usability_distribution.csv
Fig. 3c  fig3c_rwanda_hardening_dominance_heatmap.csv
Fig. 3d  fig3d_nigeria_hardening_dominance_heatmap.csv
Fig. 4   fig4_dynamics.csv                             (panels a-c)
Fig. 5a  fig5a_rwanda_tornado.csv
Fig. 5b  fig5b_nigeria_tornado.csv
Table 1  table1_extended_robustness.csv


Input data
==========

The raw survey microdata are not redistributed in this repository. To reproduce the analysis, download them from the original providers and place them in data/ as follows:

1. Rwanda Multi-Tier Framework Energy Survey, 2022 (World Bank Microdata Library; free registration required):
   https://microdata.worldbank.org/index.php/catalog/study/RWA_2022_MTF_v01_M
   Place as: data/RWA_2022_MTF_v01_M_CSV/

2. Nigeria Multi-Tier Framework Survey, 2018, North-West sample (World Bank Data Catalog / EnergyData.info):
   https://datacatalog.worldbank.org/search/dataset/0037865/
   Place as: data/NGA_2018_MTF_v01_M_CSV/
   Household weights: data/ng_weights.dta

See data/README.md for full details.


Software requirements
=====================

Python 3.10. Package versions are pinned in environment/requirements.txt. Main packages: numpy, pandas, matplotlib, seaborn, jupyter, nbconvert, ipykernel.


Running locally
===============

From the repository root:

1. Install dependencies:

   pip install -r environment/requirements.txt

2. Download the input data and place it in data/ as described above.

3. Open the notebook and run all cells from top to bottom:

   jupyter notebook code/journal_paper.ipynb

   Or execute it from the command line:

   jupyter nbconvert --to notebook --execute code/journal_paper.ipynb --output journal_paper_executed.ipynb

4. Figures and tables are written to:

   results/figures/
   results/created_csv_files/


Running with Docker
===================

From the repository root:

docker build -f environment/Dockerfile -t electrification-paper .

docker run --rm \
  -v "$PWD/data:/app/data" \
  -v "$PWD/results:/app/results" \
  electrification-paper


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

Tier 1: Lighting and phone charging.
Tier 2: Television, radio, fan, computer, DVD/VCD player, and modem/router.
Tier 3: Refrigerator, freezer, rice cooker, and food processor/blender.
Tier 4: Washing machine, electric iron, space heater, electric mill, water pump, sewing machine, and hair dryer.
Tier 5: Air conditioner, electric water heater, and microwave oven.
Tier 0: No grid or mini-grid connection. Tier 0 households are excluded from the connected-household strategy comparison.


Data-use note
=============

This repository does not redistribute any survey microdata. This repository does not imply endorsement by the World Bank, EnergyData.info, or any other data provider. The original data collectors, authorized distributors, and funding agencies bear no responsibility for the analysis, interpretations, or conclusions presented here.


Citation
========

If you use this code, please cite this repository using the citation shown on its Zenodo record, and cite the original data sources.


License
=======

Code is released under the MIT License (see LICENSE).


Contact
=======

Courage Ekoh
Golisano Institute for Sustainability
Rochester Institute of Technology
Email: ce8760@rit.edu