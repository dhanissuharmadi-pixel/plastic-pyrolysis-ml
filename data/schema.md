# Dataset schema

One row per pyrolysis experiment extracted from a published study. Fill
`dataset_template.csv` with real rows as you read papers. Keep units consistent
with the definitions below, and always record where the row came from in
`source`.

## Features

| column               | type        | unit / values                                          | notes |
|----------------------|-------------|--------------------------------------------------------|-------|
| `plastic_type`       | categorical | PE, PP, PS, PET, PVC, mixed                             | main feedstock polymer |
| `temperature_C`      | numeric     | degrees Celsius                                        | pyrolysis reaction temperature |
| `residence_time_min` | numeric     | minutes                                                | vapor/solid residence time |
| `heating_rate_C_min` | numeric     | degrees C per minute                                   | ramp rate to target temp |
| `catalyst`           | categorical | none, zeolite, HZSM-5, FCC, red_mud, other             | extend as needed |
| `catalyst_ratio`     | numeric     | catalyst:feed mass ratio (0 = none)                    | e.g. 0.1 for 1:10 |
| `reactor_type`       | categorical | fixed_bed, fluidized_bed, batch, semi_batch, screw     | extend as needed |
| `particle_size_mm`   | numeric     | millimetres                                            | feed particle size |
| `feed_mass_g`        | numeric     | grams                                                  | mass of feed charged |

## Targets

| column             | type    | unit    | notes |
|--------------------|---------|---------|-------|
| `liquid_yield_pct` | numeric | wt %    | liquid / oil / wax fraction |
| `gas_yield_pct`    | numeric | wt %    | non-condensable gas fraction |
| `solid_yield_pct`  | numeric | wt %    | char / residue fraction |

The three yields should sum to roughly 100 for a given experiment. Small
deviations (mass-balance closure < 100) are normal; the notebook checks the sum
and normalizes predictions to 100.

## Provenance

| column   | type | notes |
|----------|------|-------|
| `source` | text | paper DOI or short citation, e.g. `10.1021/acsengineeringau.2c00038` |
| `notes`  | text | anything worth remembering (assumptions, unit conversions, ranges) |

## Conventions

- One reported experiment = one row. If a paper reports a temperature sweep,
  each temperature is its own row.
- If a value is not reported, leave the cell empty rather than guessing.
- Prefer wt % for yields. If a paper reports vol %, convert or note it.
- Keep `catalyst` = `none` and `catalyst_ratio` = 0 for non-catalytic runs.
