# Carbon Intensity Report — Project Guide

## Overview

This project produces an interactive HTML report on **Australian electricity carbon intensity by state/territory**, covering approximately FY2013–14 to FY2023–24. The report includes:

- Line chart of scope 2 grid emission factors by state over time
- National average emissions intensity trend (2008–2024)
- Stacked bar chart of 2024 generation mix by state
- Per-state stacked area charts showing fuel-source contributions to emissions
- Reference bar chart of emission factors by fuel type
- Full data table with colour-coded confirmed vs interpolated values
- Assumptions, methodology notes, and numbered citations

## File Structure

```
Australia Carbon Intensity Graphs/
  carbon_intensity_report.html   # Self-contained HTML report with embedded Chart.js
data/
  scope2_emission_factors.json   # Scope 2 factors by state (FY2014–FY2024)
  national_trend.json            # National average intensity (2008–2024)
  fuel_emission_factors.json     # Emission factors by fuel type + blended gas by state
  generation_mix_2024.json       # Generation mix by state (CY2024)
  fuel_shares_by_state.json      # Fuel-type shares per state (FY2014–FY2024)
  lcoe_australia_2024.json       # LCOE ranges (GenCost 2024-25)
  lcoe_global_trends.json        # Historical global LCOE (IRENA 2010–2024)
  storage_costs.json             # Storage capital costs (GenCost 2024-25)
  heating_comparison.json        # Gas vs heat pump parameters by state
  README.md                      # Data archive documentation
references/
  lazards-lcoeplus-june-2025-_vf.pdf  # Locally stored source PDF
VERSION                          # Semantic version number
CHANGELOG.md                     # Version history and update schedule
```

The report is a single HTML file with no build step. It loads Chart.js from CDN and contains all data, charts, and styling inline. The `data/` directory mirrors all embedded data as JSON for reproducibility and link rot protection.

## How to Regenerate / Update the Report

### Updating confirmed emission factors

The most impactful improvement is replacing interpolated values with confirmed ones from official NGA Factors editions.

1. **Download NGA Factors PDFs** from DCCEEW for each year you want to confirm:
   - https://www.dcceew.gov.au/climate-change/publications/national-greenhouse-accounts-factors
   - Individual editions: 2025, 2024, 2023, 2022, 2021, 2020, and earlier
2. **Open each PDF** and find **Table 1** (or Table 5 in pre-2022 editions) — "Indirect (Scope 2) emission factors for consumption of purchased electricity"
3. **Extract the kg CO₂-e/kWh values** for each state/territory
4. **Update the `stateData` object** in the `<script>` section of `carbon_intensity_report.html`
5. **Update the `confirmedIndices` array** in the `populateTable()` function to mark newly confirmed years

### Updating generation mix data

1. **Source:** DCCEEW Australian Energy Statistics Table O — https://www.energy.gov.au/energy-data/australian-energy-statistics/electricity-generation
2. Also: Solar Calculator analysis — https://solarcalculator.com.au/blog/australian-electricity-generation-by-source/
3. Update the `genData` object in the `createGenMixChart()` function
4. Update the HTML table in section 3 ("State Generation Profiles")
5. Update the `fuelSharesByState` object for per-state stacked area charts

### Updating national trend data

1. **Sources:** Low Carbon Power (https://lowcarbonpower.org/region/Australia), Statista, Climate Change Authority, Australian Energy Council
2. Update the `nationalData` and `nationalYearsExtended` arrays

### Updating CER total emissions figures

1. **Source:** Clean Energy Regulator electricity sector data — https://cer.gov.au/markets/reports-and-data/nger-reporting-data-and-registers/
2. Update the key stats at the top of the page and citation [7]

## Key Data Sources

| Source | What it provides | URL |
|--------|-----------------|-----|
| DCCEEW NGA Factors | Official scope 2 emission factors by state (kg CO₂-e/kWh) | https://www.dcceew.gov.au/climate-change/publications/national-greenhouse-accounts-factors |
| AEMO CDEII | Daily regional NEM emissions intensity | https://www.aemo.com.au/.../carbon-dioxide-equivalent-intensity-index |
| Clean Energy Regulator | Facility-level emissions, generation, intensity by state/fuel | https://cer.gov.au/.../nger-reporting-data-and-registers/ |
| Open Electricity | Interactive NEM emissions and generation data | https://explore.openelectricity.org.au/emissions/au/ |
| energy.gov.au Table O | State generation by fuel type (annual) | https://www.energy.gov.au/energy-data/australian-energy-statistics/electricity-generation |
| Low Carbon Power | National carbon intensity and generation mix | https://lowcarbonpower.org/region/Australia |

## Data Quality Notes

- **Confirmed values** (green in table): FY2013-14 (educational reference anchor ~0.87 NSW, 1.17 VIC etc.), FY2022-23 (NGA 2023), FY2023-24 (NGA 2024)
- **Interpolated values** (yellow in table): All other years. Linearly interpolated between anchors and cross-checked against NEM-wide CDEII trends
- **Methodology change (2023):** DCCEEW stopped using 3-year rolling averages for scope 2 factors; now uses single-year data from AEMO NEM-Review
- **Fuel-source decomposition** is estimated (generation share × fuel emission factor), not directly sourced from a single dataset

## Versioning

This project uses [Semantic Versioning](https://semver.org/). The current version is in the `VERSION` file.

- **MAJOR** (e.g. 2.0.0): Breaking changes to data structure or report format
- **MINOR** (e.g. 1.1.0): New sections, charts, data sources, or features
- **PATCH** (e.g. 1.0.1): Data corrections, interpolated→confirmed upgrades, bug fixes

When releasing a new version:
1. Update `VERSION` with the new version number
2. Add an entry to `CHANGELOG.md` describing what changed
3. Update `_metadata.version` in any changed `data/*.json` files
4. Update the version in the HTML report footer

## Annual Update Schedule

The recommended annual update window is **October–November**, after key Australian data sources publish:

| Source | Typical Publication | What to Update | Priority |
|--------|-------------------|----------------|----------|
| DCCEEW NGA Factors | Aug–Oct (for prior FY) | Scope 2 emission factors, confirmed values | Critical |
| CER NGER data | Feb–Mar | Total emissions, facility data | High |
| energy.gov.au Table O | Sep–Dec | Generation mix by state/fuel | High |
| CSIRO GenCost | Dec–Jan | LCOE and storage costs | Medium |
| IRENA Cost Report | Jul–Sep | Global LCOE trends | Medium |
| Lazard LCOE+ | Jun–Jul | International LCOE benchmarks | Low |
| AEMC/AER prices | May–Jun (new FY offers) | Residential energy prices | Medium |
| Low Carbon Power | Continuous | National intensity trend | Low |

**Suggested workflow:** In October each year, check DCCEEW for the new NGA Factors edition. If published, do a full data refresh across all sources and bump a MINOR version.

## Reference Data Archive

The `data/` directory contains JSON copies of all data embedded in the HTML report. This protects against:

- **Link rot** — Government URLs change frequently; these files preserve the exact values used
- **Source revisions** — Agencies sometimes revise historical data without notice; the archive captures the values at time of retrieval
- **Reproducibility** — Anyone can verify charts or reuse data programmatically

The `references/` directory stores locally downloaded source PDFs.

When updating:
1. Update the relevant `data/*.json` file(s) — append new years, don't overwrite history
2. Update `_metadata.retrieved` and `_metadata.version`
3. Update the HTML report to match
4. If a new source PDF is available, add it to `references/`
5. Record the change in `CHANGELOG.md`

## Scope

- **Focus:** Australia only, electricity sector, scope 2 location-based emission factors
- **States covered:** NSW/ACT, VIC, QLD, SA, WA (SWIS), TAS, NT (DKIS)
- **Time range:** ~FY2013-14 to FY2023-24 (state data); 2008–2024 (national trend)
- **Units:** kg CO₂-e per kWh (emission factors); tCO₂-e per MWh (fuel factors); % (generation mix)
