# Australian Electricity Carbon Intensity by State

A self-contained, interactive HTML report on electricity grid emissions intensity across Australian states and territories, from FY2013–14 to FY2023–24.

## Quick Start

1. Open `Australia Carbon Intensity Graphs/carbon_intensity_report.html` directly in your web browser.
2. No installation, build step, or internet connection required (except to load Chart.js from CDN).
3. Charts are interactive: hover over data points, click legend items to toggle series, download as PNG.

## What's in the Report

The report contains 11 sections covering emissions intensity, generation mix, costs, and data sources:

1. **Grid Emissions Intensity Trends by State** — Scope 2 factors (kg CO₂-e/kWh) for each state over 11 years. Shows South Australia's 58% decline, Victoria's coal reliance, and Tasmania's consistent low emissions from hydro.

2. **National Emissions Intensity Trend** — Australia-wide average from 2008–2024, declining 47% (0.88 to 0.47 kg CO₂-e/kWh), driven by coal closures and renewable growth.

3. **Electricity Generation Mix by State (2024)** — Stacked bar chart and detailed table showing coal, gas, solar, wind, hydro, and other sources by state and national totals.

4. **Emissions Intensity by Fuel Source per State** — Seven stacked area charts showing fuel-type contributions to each state's grid intensity over time.

5. **Reference: Emission Factors by Fuel Type** — Bar chart of typical emissions per MWh for coal (brown/black), gas (CCGT/OCGT), oil, and zero for renewables.

6. **Levelised Cost of Energy by Technology (2024)** — Solar PV is now the cheapest source ($43–73/MWh); nuclear is the most expensive ($155–663/MWh). New coal exceeds renewables on cost alone.

7. **Historical LCOE Trends (2010–2024)** — Global weighted-average cost declines for solar (90%), wind (67%), and battery storage. Solar PV fell from $0.460/kWh to $0.043/kWh.

8. **Energy Storage Capital Costs (2024)** — Cost per kWh for lithium-ion batteries (4-hour), pumped hydro, flow batteries, and compressed air storage.

9. **Data Table** — Full table of scope 2 factors by state and financial year. Green rows = confirmed from NGA Factors 2023/2024 PDFs. Yellow rows = interpolated (verify before formal use).

10. **Assumptions & Methodology** — Explains scope 1/2/3 emissions, interpolation method, the 2023 methodology change (3-year averaging discontinued), fuel-source decomposition, and data limitations.

11. **Citations & Data Sources** — 12 primary sources (DCCEEW, AEMO, CER, CSIRO, IRENA, Lazard) with direct links to data and PDFs.

## Data Sources

| Source | Purpose | Link |
|--------|---------|------|
| **DCCEEW NGA Factors** | Official scope 2 emission factors by state (confirmed values) | https://www.dcceew.gov.au/climate-change/publications/national-greenhouse-accounts-factors |
| **AEMO CDEII** | Daily regional NEM emissions intensity | https://www.aemo.com.au/.../carbon-dioxide-equivalent-intensity-index |
| **Clean Energy Regulator** | Facility-level emissions, generation, intensity by state/fuel | https://cer.gov.au/.../nger-reporting-data-and-registers/ |
| **energy.gov.au Table O** | State generation by fuel type (annual) | https://www.energy.gov.au/energy-data/australian-energy-statistics/electricity-generation |
| **CSIRO GenCost** | LCOE by technology and storage costs | https://www.csiro.au/.../gencost |
| **IRENA** | Global LCOE trends 2010–2024 | https://www.irena.org/Energy-Transition/Technology/Power-generation-costs |
| **Low Carbon Power** | National emissions intensity trends | https://lowcarbonpower.org/region/Australia |
| **Open Electricity** | Interactive NEM emissions and generation data | https://explore.openelectricity.org.au/emissions/au/ |

## How to Update the Report

The HTML file is self-contained with all data hardcoded in the `<script>` section. To update:

### Update emission factors (FY2024–25 onwards)

1. Download the latest NGA Factors PDF from [DCCEEW](https://www.dcceew.gov.au/climate-change/publications/national-greenhouse-accounts-factors).
2. Extract Table 1 values (scope 2 factors, kg CO₂-e/kWh) for each state.
3. Add a new year to the `stateData` object in the script section (around line 363).
4. Mark the year as confirmed in the `confirmedYears` object (line 374).

### Update generation mix data

1. Source: [DCCEEW Australian Energy Statistics Table O](https://www.energy.gov.au/energy-data/australian-energy-statistics/electricity-generation) or [Solar Calculator](https://solarcalculator.com.au/blog/australian-electricity-generation-by-source/).
2. Update the `genData` object in `createGenMixChart()` function (line 570).
3. Update the HTML generation table in section 3 and the `fuelSharesByState` object (line 398).

### Update national trend data

1. Sources: [Low Carbon Power](https://lowcarbonpower.org/region/Australia), Climate Change Authority, Australian Energy Council.
2. Update `nationalYearsExtended` and `nationalData` arrays (lines 381–382).

### Update key statistics

1. Update the four key-stat boxes at the top of the page (lines 51–56).
2. Update CER total emissions figures from [Clean Energy Regulator](https://cer.gov.au/markets/reports-and-data/nger-reporting-data-and-registers/).
3. Update relevant citations and links in section 11.

## Data Quality & Scope

- **Confirmed values** (green in table): FY2013-14 (educational reference), FY2022-23 (NGA 2023), FY2023-24 (NGA 2024).
- **Interpolated values** (yellow in table): All other years. Linearly interpolated and cross-checked against NEM-wide CDEII trends. Verify against original NGA Factors PDFs before formal reporting.
- **Scope**: Australia only, electricity sector, scope 2 location-based emission factors.
- **States**: NSW/ACT, VIC, QLD, SA, WA (SWIS), TAS, NT (DKIS).
- **Time range**: FY2013-14 to FY2023-24 (state data); 2008–2024 (national trend).
- **Units**: kg CO₂-e/kWh (emission factors); tCO₂-e/MWh (fuel factors); % (generation mix); AUD/MWh (LCOE).
- **Methodology change (2023)**: DCCEEW discontinued 3-year rolling averages; factors now use single-year AEMO data. May cause small discontinuities in trends.

## Versioning & Changelog

This project uses [Semantic Versioning](https://semver.org/). See [CHANGELOG.md](CHANGELOG.md) for full version history.

- **Current version**: 1.0.0 (also in the `VERSION` file)
- **MAJOR**: Breaking changes to data structure or report format
- **MINOR**: New sections, charts, data sources, or features
- **PATCH**: Data corrections, interpolated→confirmed upgrades, bug fixes

## Reference Data Archive

The `data/` directory stores machine-readable JSON copies of all data used in the report, with full source attribution. This protects against link rot and source revisions — government URLs change frequently, and agencies sometimes revise historical data without notice.

See [data/README.md](data/README.md) for the full file listing and update instructions.

The `references/` directory stores locally downloaded source PDFs (e.g. Lazard LCOE+, GenCost reports).

## Annual Update Schedule

The recommended update window is **October–November** each year, after the most critical source (DCCEEW NGA Factors, typically Aug–Oct) publishes new scope 2 emission factors for the prior financial year.

| Source | Typical Publication | What to Update |
|--------|-------------------|----------------|
| DCCEEW NGA Factors | Aug–Oct | Scope 2 emission factors (critical) |
| CER NGER data | Feb–Mar | Total emissions figures |
| energy.gov.au Table O | Sep–Dec | Generation mix by state |
| CSIRO GenCost | Dec–Jan | LCOE and storage costs |
| IRENA Cost Report | Jul–Sep | Global LCOE trends |
| AEMC/AER prices | May–Jun | Residential energy prices |

## Technical Details

- **Format**: Single self-contained HTML file with embedded CSS and JavaScript.
- **No build step**: Just open the file in a browser.
- **Charting library**: Chart.js 4.4.1 (loaded from CDN).
- **Responsive design**: Adapts to desktop and mobile (single column below 768px).
- **Version**: 1.0.0 — 28 March 2026.

## License

Data in this report is sourced from Australian Government agencies (DCCEEW, AEMO, Clean Energy Regulator) and other authoritative sources (CSIRO, IRENA, Lazard).

Australian Government data is released under **Creative Commons Attribution 4.0 International (CC BY 4.0)**: you are free to use, modify, and distribute these materials with attribution.

See individual data source links (section 11) for specific license terms.

## Recommended Next Steps

To improve data quality:

1. Download each annual NGA Factors PDF and extract Table 1 to replace interpolated values.
2. Use the [Open Electricity API](https://docs.openelectricity.org.au/guides/emissions) to pull verified annual emissions and generation data by region and fuel type.
3. Download CER facility-level CSVs to compute precise state-level intensity by fuel source.
4. Download AEMO CDEII summary files for daily regional emissions intensity data (2011 onwards).

---

**Report**: Australian Electricity Carbon Intensity by State
**Author**: Christopher Gentle
**Generated**: 28 March 2026
**File**: `Australia Carbon Intensity Graphs/carbon_intensity_report.html`
