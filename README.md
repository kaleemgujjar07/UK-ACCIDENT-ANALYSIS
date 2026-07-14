# UK Road Accident Analysis

Comprehensive exploratory data analysis (EDA) of UK road traffic accidents from 2020–2024, analyzing over **1.2 million casualty-level records** across temporal, severity, demographic, vehicular, and spatial dimensions.

---

## 📊 Project Overview

This project merges five specialized datasets — severity, spatial, demographic, temporal, and risk factor analyses — into a single unified dataset of **1,175,663 cleaned records with 33 features**. It then explores patterns through 20+ visualizations to answer questions like:

- **When** do most accidents happen? (hour, day, month, season, year)
- **How severe** are accidents, and does timing affect severity?
- **Who** is most affected? (drivers, passengers, pedestrians — by age and sex)
- **What vehicles** are most commonly involved?
- **Where** do accidents occur? (urban vs rural, road type, speed limits)

---

## 📁 Dataset Structure

The analysis combines five pre-processed CSV files:

| File | Description | Level |
|---|---|---|
| `severity_analysis.csv` | Accident & casualty severity metrics | Casualty-level |
| `spatial_analysis.csv` | Coordinates & OS grid references | Casualty-level |
| `demographic_analysis.csv` | Age, sex, casualty class | Casualty-level |
| `temporal_analysis.csv` | Date, time, day, month, season | Accident-level |
| `risk_factors_analysis.csv` | Vehicle type, weather, road conditions | Casualty-level |

> **Source:** [UK Department for Transport — Road Safety Data](https://data.gov.uk/dataset/cb7ae6f0-4be6-4935-9277-47e5ce24a11f/road-safety-data) (2020–2024)

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Python 3** | Core language |
| **Pandas** | Data loading, merging, cleaning, transformation |
| **Seaborn** | Statistical visualizations (countplots, heatmaps, boxplots) |
| **Matplotlib** | Custom plots, annotations, formatting |
| **Statsmodels** | Time series decomposition |
| **Google Colab** | Original development environment |

---

## 🔧 Setup & Usage

### Prerequisites
```bash
pip install pandas seaborn matplotlib statsmodels
```

### Running the Script

1. **Download** the five CSV files and place them in a directory (or update the file paths in the script).
2. **Update paths** — The script defaults to Google Drive paths. Replace them with your local paths:

```python
# Change these paths to your local file locations
df_main = load_csv('./data/severity_analysis.csv')
df_spatial = load_csv('./data/spatial_analysis.csv')
df_demographic = load_csv('./data/demographic_analysis.csv')
df_temporal = load_csv('./data/temporal_analysis.csv')
df_risk = load_csv('./data/risk_factors_analysis.csv')
```

3. **Run the script:**
```bash
python uk_accident_analysis.py
```

---

## 📋 Analysis Pipeline

### 1. Data Loading & Merging
- Loads all 5 CSVs with optimized dtypes (`accident_index` as string)
- Adds sequence numbers to casualty-level files for safe multi-row merging
- Validates row count alignment across all casualty files
- Performs sequential left joins on `accident_index` + `seq`
- Merges accident-level temporal data (deduplicated)

### 2. Data Cleaning
- **22,455 exact duplicates** removed
- **Dropped columns:** `longitude`, `latitude` (insufficient data), duplicate `accident_severity_y`
- **Missing values handled:**
  - `casualty_age` (1.9%) → rows dropped
  - OS grid references (0.01%) → rows dropped
  - `driver_age` (12.4%) → retained (non-driver casualties have no driver age)
- Final clean dataset: **1,175,663 rows × 33 columns**

### 3. Exploratory Data Analysis

#### 🕐 Temporal Analysis
- Accidents by hour of day — **peak at 3–6 PM (afternoon rush)**
- Accidents by time of day — Afternoon & Evening dominate
- Accidents by day of week — **Friday highest**, weekends lowest
- Hour × Day heatmap — Weekday rush hours are clear hotspots
- Weekend vs Weekday — **3× more weekday accidents**
- Monthly & seasonal patterns — Autumn peak, February trough
- Year-over-year trends — COVID dip in 2020, stable 2022–2024
- 7-day rolling average — Reveals peak of **356 accidents/day** (late 2023)
- Monthly trend with peak/trough annotations

#### ⚠️ Severity Analysis
- Overall distribution — **76.4% Slight, 22.0% Serious, 1.5% Fatal**
- Severity by hour — Fatal/serious shift toward **late night (10 PM–2 AM)**
- Severity by day — Weekends have relatively higher serious/fatal share
- Severity by month & time of day

#### 👥 Demographic Analysis
- Casualty class distribution — **Drivers/riders > Passengers > Pedestrians**
- Class by hour — Pedestrians peak at 5–6 PM
- Casualty age by hour — Young adults (20–40) dominate, especially at night
- Driver age by hour — Late-night drivers are overwhelmingly young
- Casualty sex by hour — Males dominate at all hours

#### 🚗 Vehicle Analysis
- Top 10 vehicle types — **Cars dominate**, followed by motorcycles & bicycles
- Vehicle type by hour — Cars peak at rush hours; motorcycles/bikes in daytime
- Vehicle type by day of week

#### 📍 Time × Location Analysis
- Road type by hour — **Single carriageways** account for most accidents
- Urban vs Rural by hour — **Urban areas** have far more accidents at all hours

---

## 📈 Key Findings

| Insight | Detail |
|---|---|
| 🕒 **Rush hours are deadliest** | 7–9 AM & 4–7 PM see the most accidents |
| 🌙 **Night driving is more lethal** | Fewer crashes, but higher fatality rate (10 PM–2 AM) |
| 📉 **COVID impact** | Daily accidents dropped to **71/day** in early 2020 |
| 📊 **No long-term improvement** | Accident counts returned to pre-COVID levels by 2022 |
| 🚗 **Cars are #1** | Cars involved in the vast majority of casualties |
| 🧑 **Young drivers at night** | Late-night casualties are overwhelmingly young males |
| 🏙️ **Urban danger zones** | Cities see far more accidents than rural areas |
| 🍂 **Autumn peak** | September–November consistently worst months |

---

## 📂 Repository Structure

```
uk-accident-analysis/
├── README.md                          # This file
├── uk_accident_analysis.py            # Main analysis script
└── data/                              # (Data files not included — see Source)
    ├── severity_analysis.csv
    ├── spatial_analysis.csv
    ├── demographic_analysis.csv
    ├── temporal_analysis.csv
    └── risk_factors_analysis.csv
```

---

## 📝 License

This project is for educational and analytical purposes. The underlying dataset is provided by the UK Department for Transport under the [Open Government Licence (OGL)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).
