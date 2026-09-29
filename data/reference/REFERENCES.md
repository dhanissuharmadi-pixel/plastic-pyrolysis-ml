# Reference datasets

Public datasets pulled as reference for real value ranges, schema ideas, and
possible training rows. **Not yet cleaned into the project schema** - treat as
source material. All the Mendeley sets below are CC BY 4.0 (free to use with
attribution).

Downloaded 2026-09-20.

---

## 1. Co-pyrolysis of biomass + plastic -> bio-oil yield  ⭐ most useful

- Files: `cofiring_biomass_plastic_biooil.xls` (original), `.csv` (cleaned)
- Source: Xia, Q. (2024). *Bio-oil yield from co-pyrolysis of biomass and
  plastic.* Mendeley Data. DOI: 10.17632/xyn9bg36wr.1
- License: CC BY 4.0
- 122 rows. Yield-vs-conditions, which is exactly our problem shape.
- Columns (cleaned CSV): proximate analysis (moisture/volatile/fixed carbon/ash),
  elemental analysis of biomass and of plastic (C/H/O/N %), `residence_time_min`,
  `heating_rate_C_min`, `plastic_ratio_wpct`, `temperature_C`, `biooil_yield_wpct`.
- Ranges: temperature 300-700 C, residence 6-93 min, heating rate 5-100 C/min,
  plastic ratio 0-100 %, bio-oil yield 16.6-99.5 %.
- Note: this is co-pyrolysis (biomass + plastic), so it has feedstock chemistry
  columns our schema doesn't. Good for seeing realistic ranges and for a
  co-pyrolysis extension later.

## 2. Catalytic fast pyrolysis of PET -> crude oil

- Files: `pet_catalytic_crudeoil.xls` (original), `.csv` (cleaned)
- Source: Avalos-Ortecho, E. (2024). *Crude Oil Production from Catalytic Fast
  Pyrolysis of Waste PET and Simulation.* Mendeley Data. DOI: 10.17632/2bpdk8kwdd.1
- License: CC BY 4.0
- Temperature + pressure time-series for 6 runs (M1-M6). This is process
  monitoring data, **not** yield-vs-conditions rows. Useful for realistic
  temperature/pressure profiles, not directly for training.

## 3. Pyrolysis of mixed plastic waste (PET/HDPE + zeolite)

- Files: `avalos_pet_hdpe_zeolite.xlsx` (original), `.csv` (cleaned)
- Source: Avalos-Ortecho, E. (2025). *Optimization of Refined Fuel Production and
  Simulation from Pyrolysis of Mixed Plastic Waste.* Mendeley Data.
  DOI: 10.17632/zjkmpf5hfz.1
- License: CC BY 4.0
- Temperature + pressure time-series for 8 runs (M1-M8). Same shape as #2:
  process monitoring, not training rows.

---

## 4. Belden et al. 2023 - 325-point plastic pyrolysis dataset  ⭐ EXTRACTED

- File: `belden2023_oilyield.csv` (extracted from Table SI.1 of the SI PDF).
- The canonical open compiled dataset for this task: **325 rows from 39 papers**,
  XGBoost the best model in the paper (MAE 9.1 %). Verified row count matches.
- Source: Belden, E. R. et al. (2023). *Machine Learning Predictions of Oil
  Yields Obtained by Plastic Pyrolysis.* ACS Engineering Au 3(2), 91-101.
  DOI: 10.1021/acsengineeringau.2c00038 (open access). Data is Table SI.1 in
  `eg2c00038_si_001.pdf`. PMC mirror: https://pmc.ncbi.nlm.nih.gov/articles/PMC10119934/
- Schema (note: differs from our synthetic schema):
  - Feedstock as **mass-fraction columns** `HDPE_wt, LDPE_wt, PP_wt, PS_wt,
    PVC_wt, PET_wt` (mixture-friendly; `plastic_sum_wt` is added for transparency
    and is NOT always 100 - some rows are mixed waste with non-plastic content).
  - `temperature_C`, `heating_rate_C_min`, `particle_size_mm`, `feed_mass_g`
    (last three have missing values, left blank as in the paper).
  - `catalyst` is binary Yes/No (no ratio). `reactor_type` in {Batch, Fixed Bed,
    Fluidized Bed, Horizontal Tube, Semi Batch}.
  - **Target is `oil_yield_wpct` only** - no gas/solid split in this dataset.
  - `source_ref` = paper number 1-39 (reference list is on pp. 24-29 of the SI).
- Ranges: temperature 250-850 C, oil yield 2-99 wt%.
