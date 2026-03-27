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
```

The report is a single HTML file with no build step. It loads Chart.js from CDN and contains all data, charts, and styling inline.

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

## Scope

- **Focus:** Australia only, electricity sector, scope 2 location-based emission factors
- **States covered:** NSW/ACT, VIC, QLD, SA, WA (SWIS), TAS, NT (DKIS)
- **Time range:** ~FY2013-14 to FY2023-24 (state data); 2008–2024 (national trend)
- **Units:** kg CO₂-e per kWh (emission factors); tCO₂-e per MWh (fuel factors); % (generation mix)
