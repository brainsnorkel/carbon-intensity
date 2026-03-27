# Reference Data Archive

This directory stores machine-readable copies of all data used in the carbon intensity report. It serves two purposes:

1. **Link rot protection** — Government URLs change, PDFs get moved, and web pages get restructured. These JSON files preserve the exact values used in each report version with full source attribution.

2. **Reproducibility** — Anyone can verify the report's charts against these files, or use the data programmatically without scraping the HTML.

## Files

| File | Description | Primary Source |
|------|-------------|---------------|
| `scope2_emission_factors.json` | Scope 2 grid emission factors by state, FY2014–FY2024 | DCCEEW NGA Factors |
| `national_trend.json` | National average emissions intensity, 2008–2024 | Low Carbon Power, CCA, AEC |
| `fuel_emission_factors.json` | Emission factors by fuel type + blended gas factors by state | AEMO, NGA Factors |
| `generation_mix_2024.json` | Electricity generation mix by state, CY2024 | energy.gov.au Table O |
| `fuel_shares_by_state.json` | Fuel-type generation shares per state, FY2014–FY2024 | AEMO/OpenNEM |
| `lcoe_australia_2024.json` | LCOE ranges by technology (AUD/MWh) | CSIRO GenCost 2024-25 |
| `lcoe_global_trends.json` | Historical global LCOE for solar, wind, batteries 2010–2024 | IRENA 2024 |
| `storage_costs.json` | Energy storage capital costs (AUD/kWh) | CSIRO GenCost 2024-25 |
| `heating_comparison.json` | Gas vs heat pump parameters: grid intensity, COP, energy prices | NGA Factors, AEMC, AER |

## Structure

Each JSON file contains a `_metadata` object with:
- `title` — what the data represents
- `units` — measurement units
- `sources` — human-readable source names
- `source_urls` — URLs at time of retrieval
- `retrieved` — date the data was collected
- `version` — report version this data corresponds to

## Updating

When updating the report with new data:

1. Update the relevant JSON file(s) with new values
2. Update `_metadata.retrieved` and `_metadata.version`
3. Keep old values in the arrays (append new years, don't overwrite history)
4. Update the HTML report to match
5. Record the change in `CHANGELOG.md`

## Source PDFs

Locally stored source documents are in the `references/` directory at the project root. These provide the original context for extracted values.
