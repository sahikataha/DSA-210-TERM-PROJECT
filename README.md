# DSA-210-TERM-PROJECT — Sabancı University
## Comparative Analysis of Electric Vehicle Charging Network Operators in Istanbul

## Project Overview

This project analyzes the distribution of electric vehicle (EV) charging station operators in Istanbul using publicly available data from the Istanbul Metropolitan Municipality (İBB) Open Data Portal.

The primary aim is to understand how EV charging infrastructure is spread across the European and Asian sides of the city and to identify whether certain operators show stronger presence in specific regions. As EV adoption increases, fair access to charging stations becomes increasingly critical for sustainable urban transport planning.

The key research questions of this project are:

Which operator has the largest EV charging network in Istanbul?

How are these operators distributed across the European vs. Asian sides?

Does the overall number of charging stations significantly differ between the two sides of the city?

To answer these questions, the study applies data cleaning, exploratory data analysis (EDA), visualization, hypothesis testing, and machine learning techniques.

## Motivation

Electric vehicle usage has grown rapidly in recent years, making charging infrastructure a crucial component of sustainable mobility. In a megacity like Istanbul, where millions of people rely on urban transportation, ensuring balanced access to EV charging stations is essential.
This project aims to:

Identify whether charging stations are unevenly distributed between the European and Asian sides.
Examine whether certain operators dominate one side more than the other.
Provide data-driven insights to support decision-makers, city planners, and private companies in improving infrastructure fairness and accessibility.

## Data Sources

### Electric Vehicle Charging Stations Data
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

## Objectives of the Study

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

4. **Visualization and Reporting:**
   - Create clear visualizations using Python libraries such as matplotlib, seaborn, and scikit-learn
   - Summarize findings with interpretive commentary in a written report

5. **Machine Learning:**
   - Develop predictive models to classify charging station locations
   - Compare model performance using cross-validation

## Expected Outcomes
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
   - Selected relevant columns: `ISTASYON_NO`, `AD`, `ADRES`, `HIZMET_SEKLI`, `AGIL_ISLETMECISI_UNVAN`, `MARKA_TESCIL_BELGESI`, `LATITUDE`, `LONGITUDE`
   - Renamed columns for clarity:
     - `AGIL_ISLETMECISI_UNVAN` → `operator`
     - `MARKA_TESCIL_BELGESI` → `brand`
     - `HIZMET_SEKLI` → `service_type`

3. **Feature Engineering:**
   - Created `side` column based on longitude:
     - If `LONGITUDE < 29°`: **Europe**
     - If `LONGITUDE ≥ 29°`: **Asia**
   - This classification uses the Bosphorus Strait as the dividing line (approximately at 29°E longitude)
   - Extracted `district` names from address field using regex pattern matching
   - Created `operator_category` (Top 10 operators + "Others" for better visualization)
   - Created `operator_short` (shortened operator names for readable charts)

4. **Comprehensive Exploratory Data Analysis:**
   - Basic statistics (head, info, missing values)
   - Value counts for operators, sides, districts, brands, and service types
   - **10+ Enhanced Visualizations:**
     1. Top 10 operators bar chart (fixed from unreadable 133-operator chart)
     2. Market share pie chart with horizontal bar comparison
     3. Geographic scatter plot (Europe vs Asia with Bosphorus line)
     4. Service type analysis (Public vs Private distribution)
     5. Top 15 districts bar chart
     6. District distribution by side (stacked bar chart)
     7. Top 15 brands analysis
     8. Operator-side heatmap (Top 5 operators)
     9. Geographic density hexbin maps (separate for Europe and Asia)
     10. Comprehensive summary statistics

5. **Data Export:**
   - Saved enhanced cleaned dataset as `cleaned_stations.csv` (2,933 rows, 12 columns)

### 2. Comprehensive Hypothesis Testing (`02_hypothesis_testing.ipynb`)

**Statistical Analysis - Five Comprehensive Tests:**

#### **Test 1: Overall Europe vs Asia Distribution (Enhanced)**
- **Method:** Chi-square Goodness-of-Fit Test
- **Null Hypothesis (H₀):** Equal distribution between European and Asian sides (50-50)
- **Alternative Hypothesis (H₁):** Significant difference in distribution
- **Enhancements Added:**
  - Cramér's V effect size calculation
  - 95% Wilson confidence intervals for proportions
  - Visualization with error bars and pie chart
- **Results:**
  - Europe: 1,605 stations (54.7%)
  - Asia: 1,328 stations (45.3%)
  - χ² = 26.16, p < 0.001
  - **Conclusion:** Reject H₀ - Significant imbalance exists

#### **Test 2: Top Operators' Side Preferences (NEW)**
- **Method:** Multiple chi-square tests (one per operator)
- **Null Hypothesis (H₀):** Each operator follows overall distribution (54.7% Europe / 45.3% Asia)
- **Alternative Hypothesis (H₁):** Operator shows side preference
- **Analysis:** Tested top 5 operators individually
- **Visualizations:** Stacked bar charts and percentage comparison with significance markers

#### **Test 3: Service Type Distribution by Side (NEW)**
- **Method:** Chi-square test of independence
- **Null Hypothesis (H₀):** Service type (public/private) is independent of side
- **Alternative Hypothesis (H₁):** Service type distribution depends on side
- **Enhancements:**
  - Contingency table with expected frequencies
  - Cramér's V effect size
  - Percentage stacked bar visualization

#### **Test 4: Brand Diversity Analysis (NEW)**
- **Method:** Bootstrap test with Shannon diversity index
- **Null Hypothesis (H₀):** Brand diversity is equal on both sides
- **Alternative Hypothesis (H₁):** Brand diversity differs between sides
- **Metrics Calculated:**
  - Shannon diversity index
  - Simpson's diversity index
  - Number of unique brands per side
- **Bootstrap:** 10,000 iterations for robust p-value estimation
- **Visualizations:** Diversity metrics comparison, top brands per side, bootstrap distribution

#### **Test 5: Operator Distribution Across Districts (NEW)**
- **Method:** Chi-square test of independence
- **Null Hypothesis (H₀):** Operators are evenly distributed across districts
- **Alternative Hypothesis (H₁):** Operators show district preferences
- **Analysis:** Top 5 districts × Top 10 operators contingency table
- **Visualizations:** Heatmap and stacked bar charts showing market positioning

## Results and Findings

### Key Findings:

1. **Geographic Imbalance (Test 1):**
   - European side has **277 more stations** than the Asian side (1,605 vs 1,328)
   - This represents a **9.4% difference** in station count
   - The difference is **highly statistically significant** (p < 0.001)
   - Effect size: Cramér's V = 0.094 (small but meaningful)

2. **Operator Market Positioning (Test 2):**
   - Top 5 operators control over 35% of the market
   - Some operators show significant side preferences
   - Market is relatively concentrated with 133 total operators

3. **Service Accessibility (Test 3):**
   - Public (HALKA_ACIK) vs Private (OZEL) station distribution analyzed
   - Service type distribution patterns identified across sides

4. **Brand Diversity (Test 4):**
   - Shannon diversity indices calculated for both sides
   - Brand competition varies geographically
   - Different brands dominate different areas

5. **District Patterns (Test 5):**
   - Top districts identified: Sarıyer, Kadıköy, Beşiktaş, Şişli, Ümraniye
   - Operators show distinct geographic strategies
   - Some districts have high competition, others have monopolistic patterns

6. **Implications:**
   - **Policy:** Infrastructure planning should address European-Asian imbalance
   - **Business:** Clear market segmentation opportunities for new operators
   - **Urban Planning:** Station placement correlates with district characteristics
   - **Future Development:** Asian side needs targeted expansion
   - **Competition:** Market shows both competitive and concentrated areas

## Project Structure

```
DSA-210-TERM-PROJECT-main/
│
├── 01_data_cleaning_and_eda.ipynb     # Data cleaning and EDA notebook
├── 02_hypothesis_testing.ipynb         # Statistical hypothesis testing notebook
├── 03_machine_learning.ipynb           # Machine learning models with CV
├── cleaned_stations.csv                 # Cleaned dataset (2,933 rows)
├── requirements.txt                     # Python package dependencies
├── Elektrikli Araç Şarj İstasyonları Verisi.geojson  # Original dataset
└── README.md                            # This file
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

3. **Run the notebooks (in order):**
   - Open `01_data_cleaning_and_eda.ipynb` and run all cells
   - This will generate `cleaned_stations.csv`
   - Open `02_hypothesis_testing.ipynb` and run all cells for statistical analysis
   - Open `03_machine_learning.ipynb` and run all cells for ML models
   - View results and visualizations in the notebook outputs

### Required Packages

- `pandas` - Data manipulation and analysis
- `geopandas` - Geospatial data handling
- `matplotlib` - Data visualization
- `seaborn` - Statistical visualizations
- `scipy` - Statistical testing
- `scikit-learn` - Machine learning models
- `numpy` - Numerical computations

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

### 3. Machine Learning Models (`03_machine_learning.ipynb`)

**Predictive Modeling with Cross Validation:**

1. **Task:** Predict which side (Europe or Asia) a charging station is located on

2. **Features:** Latitude, Longitude, District (encoded), Operator (encoded)

3. **Models:**
   - **K-Nearest Neighbors (KNN):** Tested K=3,5,7,9,11,15 with StandardScaler
   - **Random Forest:** 100 trees, max_depth=10, feature importance analysis

4. **Cross Validation:** 5-Fold stratified cross validation for both models

5. **Metrics:** Accuracy, Precision, Recall, F1-Score, Confusion Matrices

6. **Visualizations:** K selection plot, confusion matrices, CV comparison, feature importance

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

- **Student:** Taha Şahika
- **Course:** DSA-210 - Data Science and Analytics
- **Institution:** Sabancı University
- **Date:** November 2025
