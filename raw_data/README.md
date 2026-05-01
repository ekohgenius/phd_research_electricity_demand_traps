README.txt

Raw data directory
==================

This folder is reserved for the input data required to reproduce the analysis in:

journal_paper.ipynb

Raw survey microdata are not included in the public repository. Users must obtain the required files directly from the original data providers and place them in the folder structure described below.

The analysis uses two main data sources:

1. Rwanda Multi-Tier Framework Energy Survey, 2022.
2. Nigeria Multi-Tier Framework survey data for the North-West Nigeria sample.


Expected folder structure
=========================

After downloading and extracting the required files, the expected structure is:

raw_data/
├── README.txt
├── ng_weights.dta
├── NGA_2018_MTF_v01_M_CSV/
│   ├── MTF_NG_HH_Identification.dta
│   ├── MTF_NG_HH_SEC_B.dta
│   ├── MTF_NG_HH_SEC_C.dta
│   ├── MTF_NG_HH_SEC_K.dta
│   ├── MTF_NG_HH_SEC_N_ELEC_ASSET.dta
│   └── [other Nigeria MTF files as needed]
└── RWA_2022_MTF_v01_M_CSV/
    ├── household_survey_data/
    │   ├── ROSTER_A1.csv
    │   ├── SECTION_B.csv
    │   ├── SECTION_C1.csv
    │   ├── SECTION_DE.csv
    │   ├── SECTION_H.csv
    │   └── [other Rwanda MTF files as needed]
    └── community_and_public_institutions_surveys_data/
        └── [Rwanda community/public institution files, if needed]


Rwanda MTF 2022 data
====================

The Rwanda analysis uses the World Bank Microdata Library dataset:

Rwanda - Multi-Tier Framework Energy Survey, 2022 (MTF 2022)

Reference ID:
RWA_2022_MTF_v01_M

DOI:
https://doi.org/10.48529/x75v-fm71

Producer:
World Bank

Version:
Edited, anonymized dataset for public distribution

The Rwanda dataset is available through the World Bank Microdata Library. Users may need to log in to the World Bank Microdata Library to download the microdata.

Recommended citation from the World Bank:

World Bank. Rwanda - Multi-Tier Framework Energy Survey, 2022 (MTF 2022). Ref: RWA_2022_MTF_v01_M. Downloaded from [uri] on [date].

Formal citation:

World Bank. (2025). Multi-Tier Framework Energy Survey, 2022 [Data set]. World Bank, Development Data Group. https://doi.org/10.48529/X75V-FM71

To reproduce the Rwanda analysis:

1. Go to the World Bank Microdata Library.
2. Search for:

   Rwanda - Multi-Tier Framework Energy Survey, 2022

3. Log in if required.
4. Download the microdata.
5. Extract the downloaded files.
6. Place the extracted folder in this directory as:

   raw_data/RWA_2022_MTF_v01_M_CSV/

The notebook expects the Rwanda household survey files to be located in:

raw_data/RWA_2022_MTF_v01_M_CSV/household_survey_data/

Key Rwanda files used by the analysis include:

ROSTER_A1.csv
SECTION_B.csv
SECTION_C1.csv
SECTION_DE.csv
SECTION_H.csv

Other files may be present in the downloaded dataset but are not necessarily required for the main analysis.


Nigeria MTF data
================

The North-West Nigeria analysis uses the Nigeria Multi-Tier Framework survey data.

Dataset name:
Nigeria - Multi-Tier Framework (MTF)

World Bank Data Catalog page:
https://datacatalog.worldbank.org/search/dataset/0037865/nigeria-multi-tier-framework-mtf

EnergyData.info dataset page:
https://energydata.info/dataset/nigeria-multi-tier-framework-mtf-survey-59

The World Bank Data Catalog page provides the Nigeria MTF dataset metadata and core survey resources. The EnergyData.info page provides access to additional survey resources used in this project, including the household survey weights file.

The North-West Nigeria household survey weights used in this analysis were obtained from the EnergyData.info dataset page and are referenced in the code as:

ng_weights.dta

To reproduce the Nigeria analysis:

1. Download the Nigeria MTF raw data from the World Bank Data Catalog:

   https://datacatalog.worldbank.org/search/dataset/0037865/nigeria-multi-tier-framework-mtf

2. Download the household survey weights file from EnergyData.info:

   https://energydata.info/dataset/nigeria-multi-tier-framework-mtf-survey-59

3. Save the weights file directly in the raw data folder as:

   raw_data/ng_weights.dta

4. Extract the Nigeria raw data and place the required files in:

   raw_data/NGA_2018_MTF_v01_M_CSV/

The notebook expects the Nigeria files to include, at minimum:

MTF_NG_HH_Identification.dta
MTF_NG_HH_SEC_B.dta
MTF_NG_HH_SEC_C.dta
MTF_NG_HH_SEC_K.dta
MTF_NG_HH_SEC_N_ELEC_ASSET.dta

The North-West Nigeria sample used in the manuscript covers seven states:

Jigawa, Kaduna, Kano, Katsina, Kebbi, Sokoto, and Zamfara.


Important file notes
====================

Do not rename the expected files unless you also update the corresponding file paths in `journal_paper.ipynb`.

The expected Nigeria folder name is:

NGA_2018_MTF_v01_M_CSV

The expected Rwanda folder name is:

RWA_2022_MTF_v01_M_CSV

The expected Nigeria weights filename is:

ng_weights.dta


Data licensing and redistribution
=================================

Rwanda
------

The Rwanda MTF 2022 data are accessed through the World Bank Microdata Library. Because the microdata require access through the World Bank platform, the raw Rwanda files are not redistributed directly in this public repository.

Users should download the Rwanda files directly from the World Bank Microdata Library and comply with the relevant terms of use.

Nigeria
-------

The Nigeria MTF dataset is listed as public and licensed under CC0 in the World Bank Data Catalog/EnergyData.info metadata. Users should still cite the original source and preserve attribution to the data provider.

Even when redistribution is permitted, users should verify the license and terms on the dataset page before redistributing the raw files.


Data-use disclaimer
===================

This repository does not imply endorsement by the World Bank, EnergyData.info, or any other data provider.

The original data collectors, authorized distributors, and funding agencies bear no responsibility for the analysis, interpretations, or conclusions presented in this project.


Troubleshooting
===============

If `journal_paper.ipynb` cannot find the data, check the following:

1. The `raw_data/` folder exists in the repository root.

2. The Rwanda folder is named exactly:

   RWA_2022_MTF_v01_M_CSV

3. The Rwanda household files are inside:

   RWA_2022_MTF_v01_M_CSV/household_survey_data/

4. The Nigeria folder is named exactly:

   NGA_2018_MTF_v01_M_CSV

5. The Nigeria weights file is named exactly:

   ng_weights.dta

6. The notebook is being run from the repository root or the paths in the notebook have been updated.

7. The files were fully extracted from the downloaded zip archives.

8. The required `.csv` and `.dta` files have not been renamed.


Contact
=======

For questions about the analysis workflow, contact:

Courage Ekoh
Golisano Institute for Sustainability
Rochester Institute of Technology

Email: ce8760@rit.edu