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

## 4. Belden et al. 2023 - 325-point plastic pyrolysis dataset (MANUAL download)

- The canonical open compiled dataset for this exact task: 325 data points from
  39 papers, XGBoost the best model (MAE 9.1 %). Features: plastic mass
  fractions, temperature, heating rate, particle size, feed amount, catalyst,
  reactor type.
- Source: Belden, E. R. et al. (2023). *Machine Learning Predictions of Oil
  Yields Obtained by Plastic Pyrolysis.* ACS Engineering Au 3(2), 91-101.
  DOI: 10.1021/acsengineeringau.2c00038 (open access)
- The data is **Table SI.1 inside the Supporting Information PDF**, and the ACS
  site is behind a Cloudflare wall that blocks scripted download. Grab it by hand:
  open the article, click "Supporting Information", download
  `eg2c00038_si_001.pdf`, and copy Table SI.1 out. Free full text (no paywall)
  is also mirrored at PMC: https://pmc.ncbi.nlm.nih.gov/articles/PMC10119934/
- Worth doing - this is the closest match to our schema and the best-known
  benchmark in the field.
