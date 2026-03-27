# Changelog

All notable changes to this project will be documented in this file.

This project uses [Semantic Versioning](https://semver.org/):
- **MAJOR**: Breaking changes to data structure or report format
- **MINOR**: New sections, charts, data sources, or features
- **PATCH**: Data corrections, interpolated→confirmed upgrades, bug fixes

## Update Schedule

Annual data refresh recommended in **October–November** each year, after key source publications:

| Source | Typical Publication | What to Update |
|--------|-------------------|----------------|
| DCCEEW NGA Factors | Aug–Oct (for prior FY) | Scope 2 emission factors, confirmed values |
| CER NGER data | Feb–Mar | Total emissions figures, facility data |
| energy.gov.au Table O | Sep–Dec | Generation mix by state/fuel |
| CSIRO GenCost | Dec–Jan | LCOE and storage cost data |
| IRENA Cost Report | Jul–Sep | Global LCOE trends |
| Lazard LCOE+ | Jun–Jul | International LCOE benchmarks |
| AEMC/AER price data | May–Jun (new FY offers) | Residential energy prices |
| Low Carbon Power | Continuous | National intensity trend |

## [1.0.0] — 2026-03-28

Initial public release.

### Added
- Scope 2 grid emission factors by state (FY2013-14 to FY2023-24) — line chart
- National emissions intensity trend (2008–2024) — line chart
- 2024 generation mix by state — stacked bar chart
- Per-state fuel-source emissions decomposition — 7 stacked area charts
- Emission factors by fuel type — reference bar chart
- LCOE by technology (GenCost 2024-25) — horizontal range bar
- Historical LCOE trends 2010–2024 (IRENA) — line chart with dual axes
- Energy storage capital costs (GenCost 2024-25) — grouped bar chart
- Gas vs heat pump vs resistive heater emissions and cost comparison — paired bar charts
- Full data table with confirmed/interpolated colour coding
- Assumptions & methodology section (scope 1/2/3 definitions)
- Citations with 14 primary and 4 supplementary sources
- Sticky navigation bar with scroll-aware active highlighting
- Slide-out table of contents panel
- Chart fullscreen/expand mode
- Mobile-responsive layout (768px breakpoint)
- GitHub Pages deployment with root redirect
- `data/` reference archive — all chart data as JSON with source attribution
- `references/` directory for locally stored source PDFs

### Data Sources (as at initial release)
- DCCEEW NGA Factors 2023, 2024, 2025 editions
- AEMO CDEII (NEM regional emissions)
- CER electricity sector emissions data
- energy.gov.au Table O (generation by fuel)
- CSIRO GenCost 2024-25 (LCOE, storage costs)
- IRENA Renewable Power Generation Costs 2024
- Lazard LCOE+ v18.0 (June 2025)
- AEMC/AER residential energy prices 2024-25
- Low Carbon Power (national intensity)
- Open Electricity / OpenNEM
