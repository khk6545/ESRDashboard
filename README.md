# East Tulsa Community Dashboard
### A Power BI Dashboard for Eastside Rise

This repository contains the data pipeline and source files for an interactive Power BI dashboard developed in partnership with [Eastside Rise](https://www.eastsiderise.org/), a community development nonprofit serving East Tulsa, Oklahoma. The dashboard was built for CUSP-GX 8133: Citizen and the City at NYU's Center for Urban Science and Progress (Spring 2026).

## Project Overview

The dashboard visualizes socioeconomic conditions in East Tulsa, compares investment disparities between East and South Tulsa, and tracks Eastside Rise program outcomes. It is designed as a public-facing transparency tool for community residents, funders, and stakeholders.

The dashboard has three pages:
- **East Tulsa Snapshot** — Income, poverty, homeownership, housing burden, and community context indicators
- **Small Business & Investment Gap** — PPP and Restaurant Revitalization Fund capital distribution, workforce occupation breakdown, and self-employment rates compared across East and South Tulsa
- **Community Impact** — N-400 Citizenship Fee Reimbursement Program outcomes, with placeholder sections for Housing Repair and Rental Assistance programs

## Dashboard Preview

**Page 1 — East Tulsa Snapshot**
![East Tulsa Snapshot](Snapshot%202.png)

**Page 2 — Small Business & Investment Gap**
![Small Business & Investment Gap](Business%202.png)

**Page 3 — Community Impact**
![Community Impact](Impact%202.png)

## Getting Started

### Requirements
- [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) (free download)
- Microsoft Excel (to view source data)

### Setup Instructions

1. **Download the repository**
   Clone or download the repository as a ZIP file and extract it to a folder on your computer.

2. **Open the dashboard**
   Double-click `ESR_Dashboard.pbix` to open it in Power BI Desktop. You will immediately see a data source error — this is expected. The dashboard needs to be pointed to the location where you saved `ESR_Compiled.xlsx`.

3. **Reconnect the data source**
   - In Power BI Desktop, go to **Home → Transform Data → Data Source Settings**
   - You will see `ESR_Compiled.xlsx` listed with an error or a path that no longer exists
   - Click on it → select **Change Source**
   - Navigate to the folder where you saved `ESR_Compiled.xlsx` and select it
   - Click **OK** → **Close**

4. **Refresh the data**
   - Back in the main Power BI window, click **Home → Refresh**
   - Wait for all tables to load — this may take 30–60 seconds
   - The dashboard should now display correctly

5. **Verify the connection**
   - Check that all three pages display data correctly
   - If any visuals are still blank, repeat step 3 and confirm you selected the correct file

### Notes
- Keep `ESR_Compiled.xlsx` and `ESR_Dashboard.pbix` in the same folder for simplicity
- Do not rename any sheets in `ESR_Compiled.xlsx` — Power BI connects to sheets by their exact name
- If you move `ESR_Compiled.xlsx` to a different folder after setup, repeat step 3 to update the path

## Data Sources

| Source | Description |
|---|---|
| U.S. Census Bureau ACS 5-Year Estimates (2019–2023, 2020–2024) | Income, poverty, housing, employment, and occupation data at ZIP code level |
| SBA Paycheck Protection Program FOIA Data (2020–2021) | Small business COVID relief loan distribution by ZIP code |
| SBA Restaurant Revitalization Fund FOIA Data (2021) | Restaurant-specific COVID relief grant distribution |
| City of Tulsa Open Data | Property values, code violations, and neighborhood condition data |
| Eastside Rise Internal Program Data | N-400 Citizenship Fee Reimbursement participant records (2024–2026) |

## Repository Contents

```
├── ESR_Compiled.xlsx                        # All source data — primary input to Power BI
├── ESR_Dashboard.pbix                       # Power BI dashboard file
├── ESR_Dashboard_Management_Guide.docx      # Staff guide for maintaining and updating the dashboard
└── README.md
```

## Geography

East Tulsa is defined by ZIP codes **74128** and **74129** based on Eastside Rise's service area definition. South Tulsa comparison areas are ZIP codes **74105** (South Brookside) and **74137** (Hunter Park), selected to represent different income levels within South Tulsa while remaining geographically comparable to East Tulsa.

## Methods

- All Census data uses ACS 5-year pooled estimates — not year-over-year snapshots. Figures represent baseline conditions rather than trends.
- PPP and RRF capital figures reflect the COVID-era investment gap and are static (programs closed).
- N-400 fee reimbursement totals are estimated at $720 per participant (80% online/$710, 20% paper/$760 weighted average).
- The data model uses a star schema with `ACS_Summary_Comparison` as the central table connected to all other tables via ZIP code.

## Data Model

The dashboard uses a star schema with `ACS_Summary_Comparison` as the central dimension table. All other tables connect to it via ZIP code in a Many-to-One relationship with single cross-filter direction.

| Table | Relationship | Notes |
|---|---|---|
| `ACS_S1901_2023_ZIPs` | ZIP → ACS_Summary_Comparison | Median household income |
| `ACS_S1701_2023_ZIPs` | ZIP → ACS_Summary_Comparison | Poverty rate |
| `ACS_DP03_2023_ZIPs` | ZIP → ACS_Summary_Comparison | Employment counts |
| `ACS_DP04_2023_ZIPs` | ZIP → ACS_Summary_Comparison | Housing characteristics |
| `DP03_Employment` | ZIP → ACS_Summary_Comparison | Labor force participation |
| `DP03_Occupation` | ZIP → ACS_Summary_Comparison | Occupation breakdown |
| `DP03_Industry` | ZIP → ACS_Summary_Comparison | Industry breakdown |
| `DP03_Income_Poverty` | ZIP → ACS_Summary_Comparison | Income distribution |
| `DP03_Health_Insurance` | ZIP → ACS_Summary_Comparison | Insurance coverage |
| `PPP Loans - Tulsa` | ZIP → ACS_Summary_Comparison | Raw PPP loan records |
| `PPP Summary` | ZIP → ACS_Summary_Comparison | Aggregated PPP totals |
| `RRF_Tulsa_Clean` | ZIP → ACS_Summary_Comparison | RRF grant records |
| `Naturalization addresses` | ZIP → ACS_Summary_Comparison | N-400 program participants |

## Built With

- **Power BI Desktop** — dashboard development
- **Microsoft Excel** — data compilation and cleaning
- **Python** — data processing and file generation
- **U.S. Census Bureau API** — supplemental data pulls
