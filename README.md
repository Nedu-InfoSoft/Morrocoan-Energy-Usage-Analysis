# Morocco Energy Data Analysis

A full data analysis project covering energy load data from four cities in Morocco. The project reads 10-minute interval readings from multiple measurement zones across Laayoune, Boujdour, Foum eloued, and Marrakech. It produces statistics, seasonal patterns, zone comparisons, anomaly detection, and five visual charts.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Dataset Description](#dataset-description)
- [Cities and Zones](#cities-and-zones)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Installation](#installation)
- [How to Run](#how-to-run)
- [Analysis Covered](#analysis-covered)
- [Charts Produced](#charts-produced)
- [Key Findings](#key-findings)
- [Limitations](#limitations)
- [Future Work](#future-work)
- [License](#license)

---

## Project Overview

This project takes a single Excel file (`Data_Morocco.xlsx`) with four tabs and runs a full analysis on it using Python. The goal is to understand energy demand patterns across four Moroccan cities. The analysis looks at how load changes by hour, day, month, and season. It also checks which zones behave similarly, where anomalies occur, and where zero readings may point to outages or sensor issues.

Everything runs from one script: `morocco_analysis.py`. You do not need to clean or prepare the data before running it. The script handles all of that.

---

## Dataset Description

**File name:** `Data_Morocco.xlsx`

The file has four sheets, one for each city. Each sheet has the same basic structure:

| Column | Description |
|---|---|
| `DateTime` | Timestamp for the reading, recorded every 10 minutes |
| `zone1` | Load or consumption reading for Zone 1 |
| `zone2` | Load or consumption reading for Zone 2 |
| `zone3` and beyond | Additional zones depending on the city |

- Readings are taken every 10 minutes
- All timestamps are in `datetime64` format
- All zone values are numeric (float or integer)
- No missing values were found in any of the four sheets

---

## Cities and Zones

| City | Number of Zones | Total Rows | Date Range |
|---|---|---|---|
| Laayoune | 5 | 88,890 | September 2022 to May 2024 |
| Boujdour | 3 | 88,890 | September 2022 to May 2024 |
| Foum eloued | 7 | 88,890 | September 2022 to May 2024 |
| Marrakech | 2 | 17,501 | January 2023 to January 2024 |

Laayoune, Boujdour, and Foum eloued share the same time range and row count. Marrakech covers a shorter period and has fewer rows.

---

## Project Structure

```
morocco-energy-analysis/
│
├── Data_Morocco.xlsx         # The raw data file (4 city tabs)
├── morocco_analysis.py       # Main analysis script
├── README.md                 # This file
│
└── outputs/
    ├── fig1_overview.png     # Overview dashboard
    ├── fig2_zones.png        # Zone averages per city
    ├── fig3_correlations.png # Zone correlation heatmaps
    ├── fig4_patterns.png     # Anomalies, weekday vs weekend, seasons
    └── fig5_outages.png      # Zero reading analysis
```

---

## Requirements

You need Python 3.8 or higher. The following Python libraries are required:

| Library | Purpose |
|---|---|
| `pandas` | Reading the Excel file and all data processing |
| `numpy` | Math operations used in normalization and anomaly detection |
| `matplotlib` | Drawing all charts and figures |
| `seaborn` | Drawing the correlation heatmaps |
| `openpyxl` | Backend used by pandas to read `.xlsx` files |

---

## Installation

**Step 1: Clone the repository**

```bash
git clone https://github.com/your-username/morocco-energy-analysis.git
cd morocco-energy-analysis
```

**Step 2: Create a virtual environment (recommended)**

```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

**Step 3: Install all required libraries**

```bash
pip install pandas numpy matplotlib seaborn openpyxl
```

Or if you have a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

**requirements.txt content:**

```
pandas
numpy
matplotlib
seaborn
openpyxl
```

---

## How to Run

Make sure `Data_Morocco.xlsx` is in the same folder as `morocco_analysis.py`. Then run:

```bash
python morocco_analysis.py
```

The script will:

1. Load all four city tabs from the Excel file
2. Print statistics for each city to the terminal
3. Save five chart images to your current working folder

The whole script runs in under one minute on a normal computer.

---

## Analysis Covered

The script runs the following analyses on every city tab:

**Basic Summary**
- Row count, column names, and data types
- Date range (first and last timestamp)
- Descriptive statistics: mean, min, max, standard deviation, and percentiles for every zone

**Missing Data Check**
- Counts how many values are missing in each column
- All four cities had zero missing values

**Time-Based Patterns**
- Hourly averages: what is the average total load for each hour of the day (0 to 23)?
- Daily averages: grouped by day of the week
- Monthly averages: grouped by month number (1 to 12)
- Seasonal averages: grouped into Autumn, Spring, Summer, and Winter

**Weekday vs Weekend**
- Compares average total load on weekdays (Monday to Friday) against weekends (Saturday and Sunday)

**Year-over-Year Totals**
- Adds up total load for each calendar year present in the data

**Zone Correlation**
- A correlation matrix showing how closely each pair of zones moves together
- Values near 1.0 mean the zones go up and down at the same time
- Values near 0 mean the zones are independent
- Negative values mean the zones move in opposite directions

**Anomaly Detection**
- Flags any reading that is more than 3 standard deviations above the mean for that zone
- These are potential demand spikes, sensor errors, or unusual events

**Zero Reading Analysis**
- Counts how many times each zone recorded exactly 0
- Shows this as a percentage of all readings
- Zero readings can mean outages, sensor downtime, or maintenance periods

**Peak Moments**
- Shows the top 5 highest total load readings ever recorded for each city
- Includes the exact timestamp and the value of every zone at that moment

---

## Charts Produced

### Chart 1: Overview Dashboard (`fig1_overview.png`)

Four panels in one image:

- **Top left:** Average total load per city as a bar chart
- **Top right:** Number of zones per city
- **Middle:** Hourly load profile for all four cities on one line chart. Values are normalized (0 to 100) so cities with very different sizes can be compared on the same scale
- **Bottom:** Monthly load profile for all four cities as grouped bars, also normalized

### Chart 2: Zone Averages (`fig2_zones.png`)

One panel per city. Each bar represents one zone and shows the average load. Error bars show one standard deviation above and below the average. This makes it easy to see which zones are busy and which ones vary a lot.

### Chart 3: Zone Correlation Heatmaps (`fig3_correlations.png`)

One heatmap per city. Each box shows the correlation between two zones. Red means a strong positive link. Blue means a negative link. Numbers inside each box show the exact value. This chart reveals the internal structure of each city's zone network.

### Chart 4: Patterns Dashboard (`fig4_patterns.png`)

Three panels:

- **Top left:** Total anomaly count per city (bars)
- **Top right:** Weekday vs weekend average load comparison for all four cities (grouped bars)
- **Bottom:** Seasonal load expressed as a percentage of each city's own average. The dashed line at 100% is the city's baseline. Bars above 100% mean that season is above average. This makes seasonal effects easy to spot and compare across cities.

### Chart 5: Zero Reading Analysis (`fig5_outages.png`)

One horizontal bar chart per city. Each bar shows what percentage of that zone's readings were exactly zero. A red dashed line marks the 1% threshold. Any zone crossing this line deserves closer attention.

---

## Key Findings

**Load levels**
Marrakech has the highest load per zone by a very large margin. Zone 1 in Marrakech averages over 1,200 units per reading. The next highest single zone is Zone 3 in Laayoune at around 151 units. This suggests Marrakech zones cover much larger areas or higher-demand facilities.

**Peak times**
Laayoune and Boujdour both peak at 21:00. Foum eloued peaks at 19:00. Marrakech peaks at 15:00 in the afternoon. This mid-afternoon peak in Marrakech is different from the evening peaks in the other three cities and may reflect commercial or industrial demand.

**Seasonal patterns**
Autumn is the highest season for Laayoune, Boujdour, and Foum eloued. Marrakech has its highest demand in Summer and Autumn, likely due to air conditioning in its hotter inland climate. Summer is the lowest season for the three coastal cities.

**Weekday vs weekend**
Foum eloued has the biggest drop on weekends: about 16% lower than weekdays. The other cities show much smaller differences. This suggests Foum eloued has more activity tied to working days.

**Zone structure**
In Laayoune, Zone 1 barely links to the other four zones. Zones 2 to 5 are all strongly connected to each other. In Boujdour, Zone 2 is negatively linked to Zone 1, suggesting it may act as a backup or a separate supply source. In Foum eloued, Zones 5 and 6 behave very differently from the rest. Zone 6 is negatively correlated with all other zones.

**Anomalies**
Foum eloued has the most anomalies overall (2,026 total). Zone 2 in Boujdour has 713 anomalies alone, making it the most unstable single zone in the dataset. Marrakech has the fewest anomalies (67 total).

**Outages**
Only one zone crosses the 1% zero-reading threshold: Zone 4 in Foum eloued at 1.31%. All other zones are below 1%, which is within an acceptable range.

**Record highs**
- Laayoune: 1,317 units on 20 January 2024 at 14:30
- Boujdour: 278 units on 31 October 2022 at 18:40
- Foum eloued: 948 units on 18 October 2022 at 20:50
- Marrakech: 2,559 units on 11 August 2023 at 17:30

The Marrakech peak on a hot August afternoon is very clear evidence of summer cooling demand.

---

## Limitations

- The dataset does not include labels for what each zone measures. The analysis assumes they are all load or consumption readings of the same type.
- Marrakech covers a shorter time range than the other three cities. Year-over-year comparisons for Marrakech are therefore less reliable.
- Zero readings are treated as potential outages but could also be legitimate low-demand periods or scheduled shutdowns.
- Anomaly detection uses a simple statistical method (mean plus 3 standard deviations). This may miss gradual drift or classify normal seasonal peaks as anomalies in some cases.
- The analysis does not factor in public holidays, which could explain some of the unusual patterns.

---

## Future Work

The following would make this project stronger:

- Add labels for what each zone represents (residential, commercial, industrial)
- Include weather data (temperature, humidity) to model the link between climate and demand
- Build a forecasting model to predict future load based on historical patterns
- Add an interactive dashboard using a tool like Plotly or Streamlit
- Automate anomaly alerts so that any reading crossing a threshold sends a notification
- Compare the four cities against each other on a map to show geographic demand patterns

---

## License

This project is open for personal and research use. If you use this analysis or script in your own work, please give credit to the original source.

---

*Built with Python, pandas, matplotlib, seaborn, and numpy.*
