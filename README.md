DSA-210-TERM-PROJECT — Sabancı University
Comparative Analysis of Electric Vehicle Charging Network Operators in Istanbul

Project Overview

This project analyzes the distribution of electric vehicle (EV) charging station operators in Istanbul using publicly available data from the Istanbul Metropolitan Municipality (İBB) Open Data Portal.

The primary aim is to understand how EV charging infrastructure is spread across the European and Asian sides of the city and to identify whether certain operators show stronger presence in specific regions. As EV adoption increases, fair access to charging stations becomes increasingly critical for sustainable urban transport planning.

The key research questions of this project are:

Which operator has the largest EV charging network in Istanbul?

How are these operators distributed across the European vs. Asian sides?

Does the overall number of charging stations significantly differ between the two sides of the city?

To answer these questions, the study applies data cleaning, exploratory data analysis (EDA), visualization, and hypothesis testing techniques.

Motivation

Electric vehicle usage has grown rapidly in recent years, making charging infrastructure a crucial component of sustainable mobility. In a megacity like Istanbul, where millions of people rely on urban transportation, ensuring balanced access to EV charging stations is essential.
This project aims to:

Identify whether charging stations are unevenly distributed between the European and Asian sides.
Examine whether certain operators dominate one side more than the other.
Provide data-driven insights to support decision-makers, city planners, and private companies in improving infrastructure fairness and accessibility.

Data Sources
Electric Vehicle Charging Stations Data
The dataset is obtained from the İBB Open Data Portal and includes:

Charging station name
Operator
Latitude and longitude
Region information (district and neighborhood)
Operational status
The dataset contains no personal or sensitive information and is provided in GeoJSON format.

**Dataset Statistics:**
- Total number of charging stations: **2,933**
- Format: GeoJSON (FeatureCollection)
- Original file: `Elektrikli Araç Şarj İstasyonları Verisi.geojson`
Objectives of the Study

1. Operator Distribution Analysis
Count the number of stations per operator.
Compare operators’ overall presence in Istanbul.
Identify which operators have stronger concentration on each side of the city.

2. Spatial Pattern Analysis
Map station locations by latitude and longitude.
Compare the density of stations between the European and Asian sides.
Visualize regional differences using geographic plots.

3. Statistical Analysis
Conduct hypothesis tests to determine whether the number of charging stations differs significantly between the two sides of the city.
Examine whether operator presence shows side-based variation.

4. Visualization and Reporting
Create clear visualizations using Python libraries such as matplotlib, seaborn, and folium.
Summarize findings with interpretive commentary in a written report.

Expected Outcomes
The project is expected to deliver:

Identification of the main EV charging operators in Istanbul.
Visual comparisons of operator presence on the European and Asian sides.
Geographic visualizations showing the distribution of EV stations.
A hypothesis test revealing whether the two sides differ significantly in charging infrastructure density.
Practical insights to support more equitable infrastructure planning.

## Methodology

### 1. Data Cleaning and Preprocessing (`01_data_cleaning_and_eda.ipynb`)

**Steps Performed:**

1. **Data Loading:**
   - Read GeoJSON file using `geopandas`
   - Initial dataset contains 2,933 charging stations

2. **Column Selection:**
   - Selected relevant columns: `ISTASYON_NO`, `AD`, `ADRES`, `AGIL_ISLETMECISI_UNVAN`, `MARKA_TESCIL_BELGESI`, `LATITUDE`, `LONGITUDE`
   - Renamed columns for clarity:
     - `AGIL_ISLETMECISI_UNVAN` → `operator`
     - `MARKA_TESCIL_BELGESI` → `brand`

3. **Feature Engineering:**
   - Created `side` column based on longitude:
     - If `LONGITUDE < 29°`: **Europe**
     - If `LONGITUDE ≥ 29°`: **Asia**
   - This classification uses the Bosphorus Strait as the dividing line (approximately at 29°E longitude)

4. **Exploratory Data Analysis:**
   - Basic statistics (head, info, missing values)
   - Value counts for operators and sides
   - Visualizations:
     - Bar chart: Number of stations per operator
     - Bar chart: Europe vs Asia distribution

5. **Data Export:**
   - Saved cleaned dataset as `cleaned_stations.csv` (2,933 rows, 8 columns)

### 2. Hypothesis Testing (`02_hypothesis_testing.ipynb`)

**Statistical Analysis:**

1. **Hypothesis Formulation:**
   - **H₀ (Null Hypothesis):** There is no significant difference in the number of charging stations between the European and Asian sides of Istanbul.
   - **H₁ (Alternative Hypothesis):** There is a significant difference in the number of charging stations between the two sides.

2. **Test Selection:**
   - **Chi-square Goodness-of-Fit Test**
   - Compares observed distribution (Europe vs Asia) against expected equal distribution (50-50)
   - Significance level: α = 0.05

3. **Results:**
   - **Europe:** 1,605 stations (54.7%)
   - **Asia:** 1,328 stations (45.3%)
   - **Chi-square statistic:** 26.16
   - **P-value:** < 0.000001
   - **Conclusion:** Reject H₀ - Significant difference detected

## Results and Findings

### Key Findings:

1. **Geographic Distribution:**
   - European side has **277 more stations** than the Asian side
   - This represents a **9.4% difference** in station count
   - The difference is **statistically significant** (p < 0.05)

2. **Operator Analysis:**
   - Multiple operators serve Istanbul's EV charging network
   - Distribution varies by operator (see visualizations in notebook)

3. **Implications:**
   - Infrastructure planning should consider the imbalance between sides
   - Future development may need to focus on increasing Asian side coverage
   - Current distribution may reflect population density, economic activity, or historical development patterns

## Project Structure

```
DSA-210-TERM-PROJECT-main/
│
├── 01_data_cleaning_and_eda.ipynb    # Data cleaning and EDA notebook
├── 02_hypothesis_testing.ipynb        # Statistical hypothesis testing notebook
├── cleaned_stations.csv                # Cleaned dataset (2,933 rows)
├── requirements.txt                    # Python package dependencies
├── Elektrikli Araç Şarj İstasyonları Verisi.geojson  # Original dataset
└── README.md                           # This file
```

## Installation and Usage

### Prerequisites

- Python 3.7 or higher
- Jupyter Notebook or JupyterLab

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/sahikataha/DSA-210-TERM-PROJECT.git
   cd DSA-210-TERM-PROJECT
   ```

2. **Install required packages:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the notebooks:**
   - Open `01_data_cleaning_and_eda.ipynb` and run all cells
   - This will generate `cleaned_stations.csv`
   - Open `02_hypothesis_testing.ipynb` and run all cells
   - View results and visualizations in the notebook outputs

### Required Packages

- `pandas` - Data manipulation and analysis
- `geopandas` - Geospatial data handling
- `matplotlib` - Data visualization
- `seaborn` - Statistical visualizations
- `scipy` - Statistical testing

## Technical Details

### Data Processing Pipeline

1. **Input:** GeoJSON file with 2,933 features
2. **Processing:**
   - Column selection and renaming
   - Geographic classification (Europe/Asia)
   - Data validation and cleaning
3. **Output:** Cleaned CSV file ready for analysis

### Statistical Test Details

- **Test Type:** Chi-square Goodness-of-Fit Test
- **Degrees of Freedom:** 1
- **Expected Distribution:** Equal (50-50)
- **Observed Distribution:** 54.7% Europe, 45.3% Asia
- **Test Statistic:** 26.16
- **P-value:** < 0.000001
- **Interpretation:** Strong evidence against null hypothesis

## Future Work

Potential extensions of this analysis:

1. **Operator-Specific Analysis:** Test if specific operators show side-based preferences
2. **Temporal Analysis:** Track changes in distribution over time
3. **Spatial Clustering:** Identify hotspots and coverage gaps
4. **Demographic Correlation:** Compare station distribution with population density
5. **Accessibility Analysis:** Evaluate station accessibility by public transportation

## Ethical Considerations

All data used in this study are public, aggregated, and non-personal. No individual-level or sensitive information is included in the analysis. The dataset is obtained from publicly available sources (İBB Open Data Portal) and contains only infrastructure location data.

## License

This project is for educational purposes as part of the DSA-210 course at Sabancı University.

## Author

- **Student:** Taha Ardahanlı
- **Course:** DSA-210 - Data Science and Analytics
- **Institution:** Sabancı University
- **Date:** November 2024
