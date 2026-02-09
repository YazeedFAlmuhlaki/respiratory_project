# King County Adult Asthma Prevalence: A Spatial Data Science Analysis

**Author:** Yazeed Almuhlaki  
**Date:** February 2026  
**Study Area:** King County, Washington (Seattle Metropolitan Area)

---

## Executive Summary

This study examines the spatial distribution of adult asthma prevalence across 494 census tracts in King County, Washington, and its relationship with environmental exposures including industrial pollution, traffic density, and green space coverage. Using spatial statistical methods including Global Moran's I, Local Indicators of Spatial Association (LISA), and spatial regression modeling, we investigated whether environmental exposures predict asthma prevalence after controlling for socioeconomic factors.

**Key Findings:**
- Asthma prevalence exhibits very strong spatial clustering (Moran's I = 0.608, p < 0.001)
- 191 statistically significant clusters identified: 83 high-high hotspots, 95 low-low coldspots
- After spatial correction, industrial pollution proximity does not significantly predict asthma (p = 0.114)
- Socioeconomic factors (poverty, education) remain robust predictors
- The urban-rural health paradox persists: high asthma in low-pollution rural areas, low asthma in high-pollution urban areas

---

## Repository Structure
```
respiratory_project/
├── data/
│   ├── raw/                    # Original data sources
│   └── processed/              # Cleaned and engineered datasets
|             
├── notebooks/
│   ├── analysis.ipynb          # analysis notebook
│   ├── data_preparation.ipynb  # data enginnering notebook
| 
├── outputs/                   # color maps             
├── requirements.txt
└── README.md
```

---

## Data Sources

| Dataset | Source | Description |
|---------|--------|-------------|
| Health Outcome | CDC PLACES 2024 | Model-based adult asthma prevalence estimates |
| Census Boundaries | U.S. Census Bureau TIGER/Line 2020 | Census tract polygon geometries (n=494) |
| Industrial Pollution | EPA TRI 2024 | Toxic Release Inventory facility locations and emissions |
| Road Network | TIGER/Line / OpenStreetMap | Major road segments (motorway through tertiary) |
| Green Space | OpenStreetMap | Parks, forests, grassland, recreation areas |
| Socioeconomic Controls | U.S. Census ACS 2018-2022 | Poverty, race/ethnicity, educational attainment |

---

## Methodology

### Spatial Data Processing
- Projected coordinate system: NAD83 Washington State Plane South (EPSG:2926)
- Spatial operations: buffer analysis, distance calculations, area computations
- Feature engineering: TRI proximity metrics, road density, green space percentage

### Exploratory Spatial Data Analysis (ESDA)
- Choropleth mapping with natural breaks classification
- Global Moran's I statistic (999 permutations) for spatial autocorrelation testing
- Local Indicators of Spatial Association (LISA) for hotspot identification
- Queen contiguity spatial weights (row-standardized)

### Statistical Modeling
- Ordinary Least Squares (OLS) regression baseline
- Variance Inflation Factor (VIF) diagnostics for multicollinearity
- Residual Moran's I testing for spatial autocorrelation in errors
- Spatial Error Model (GM estimator) to correct for spatial dependence

---

## Key Results

### Descriptive Statistics

| Variable | Min | Mean | Median | Max | SD |
|----------|-----|------|--------|-----|-----|
| Asthma prevalence (%) | 7.3 | 9.8 | 9.9 | 13.9 | 0.95 |
| Distance to TRI (km) | 0.0 | 2.9 | 2.4 | 22.1 | 2.67 |
| Road density (km/km²) | 0.3 | 11.1 | 10.3 | 31.9 | 5.67 |
| Green space (%) | 0.0 | 16.3 | 11.1 | 87.4 | 15.89 |
| Percent low income | 0.0 | 5.8 | 3.6 | 100.0 | 8.13 |

### Spatial Autocorrelation
- Global Moran's I: 0.608 (z = 23.60, p < 0.001)
- Strong positive spatial autocorrelation confirmed
- 38.7% of tracts in statistically significant local clusters

### Regression Model Comparison

| Variable | OLS Coefficient | OLS p-value | Spatial Error Coefficient | Spatial Error p-value |
|----------|----------------|-------------|--------------------------|----------------------|
| Low income (%) | +0.042 | <0.001 | +0.030 | <0.001 |
| Minority (%) | -0.013 | <0.001 | -0.004 | 0.085 |
| Less than HS education (%) | +0.137 | <0.001 | +0.055 | <0.001 |
| Distance to TRI (km) | +0.041 | 0.007 | +0.030 | 0.114 |
| Road density (km/km²) | -0.030 | <0.001 | -0.024 | 0.003 |
| Green space (share 0-1) | -0.536 | 0.062 | -0.822 | 0.001 |
| Lambda (spatial error) | — | — | 0.661 | <0.001 |

**Model diagnostics:**
- OLS residual Moran's I: 0.477 (p < 0.001) - significant spatial autocorrelation
- All predictor VIF values < 5 - no multicollinearity concerns
- Lambda = 0.661 indicates 66% of error variance explained by spatial dependence

---

## Main Findings

### The Urban-Rural Health Paradox
The highest asthma prevalence occurs in rural eastern King County, characterized by low industrial pollution, minimal traffic, and abundant green space. Conversely, Seattle's urban core exhibits the lowest asthma prevalence despite proximity to industrial facilities, high traffic density, and limited green space. This paradox persists even after controlling for socioeconomic factors and spatial autocorrelation.

### Industrial Pollution: Not a Significant Predictor
After spatial correction, proximity to TRI facilities does not significantly predict asthma prevalence (p = 0.114). The apparent relationship in OLS (p = 0.007) was driven by spatial confounding rather than causal exposure.

### Socioeconomic Determinants Dominate
Poverty and low educational attainment remain significant predictors after spatial correction, though effect sizes are substantially reduced. The minority percentage association disappears after spatial correction, revealing it as a spatial artifact.

### Traffic Exposure Paradox
Road density maintains a significant inverse association with asthma (p = 0.003) even after spatial correction, confirming the urban-rural health gradient.

### Green Space Protective Effect
Green space shows a significant protective association (p = 0.001) after spatial correction, though interpretation requires caution due to potential confounding between rural forests and urban parks.

---

## Policy Implications

1. **Target geographic hotspots:** Public health interventions should focus on the 83 identified hotspot tracts in rural eastern King County
2. **Address socioeconomic determinants:** Poverty and education are stronger predictors than environmental pollution exposure
3. **Improve healthcare access:** Rural-urban disparities in asthma may reflect healthcare access rather than pollution exposure
4. **Reassess pollution regulations:** Industrial pollution proximity regulations may have limited impact on asthma disparities compared to investments in healthcare infrastructure and social determinants

---

## Limitations

- **Ecological fallacy:** Tract-level associations do not establish individual-level causal relationships
- **Temporal alignment:** Current exposure proxies may not reflect historical exposures relevant to asthma development
- **Unmeasured confounders:** Indoor air quality, smoking rates, occupational exposures, and healthcare utilization not modeled
- **Measurement uncertainty:** CDC PLACES asthma estimates are model-based with varying uncertainty across tracts
- **Exposure proxies:** Road density is not traffic volume; green space does not distinguish park accessibility or quality
- **MAUP sensitivity:** Results may vary with different spatial aggregation units

---

## Technical Stack

**Languages & Core Libraries:**
- Python 3.13
- GeoPandas 1.0+ (spatial data manipulation)
- PySAL/esda (spatial autocorrelation analysis)
- PySAL/spreg (spatial regression modeling)
- statsmodels (OLS regression, diagnostics)

**Visualization:**
- matplotlib (static maps and plots)
- contextily (basemap integration)


**Data Processing:**
- pandas (tabular data)
- NumPy (numerical operations)
- shapely (geometric operations)

---

## Reproducibility

All analyses are fully reproducible. The workflow proceeds through four main notebooks:

1. **Data Preparation:** Download and clean raw datasets, establish coordinate reference systems
2. **Feature Engineering:** Create TRI proximity metrics, road density, green space coverage
3. **Exploratory Analysis:** Generate descriptive statistics, maps, and spatial autocorrelation tests
4. **Spatial Regression:** Fit OLS baseline, conduct diagnostics, estimate spatial error model

Spatial weights specification:
- Type: Queen contiguity (tracts sharing any boundary point)
- Transformation: Row-standardized
- Properties: 494 tracts, average 6.07 neighbors, no islands

---

## Future Work

- Investigate healthcare access patterns and asthma management program availability
- Assess indoor air quality in rural housing (woodsmoke, mold, ventilation)
- Incorporate historical exposure data and longitudinal health outcomes
- Examine individual-level exposure-outcome relationships using geocoded health records
- Extend analysis to other respiratory conditions (COPD, bronchitis)
- Develop predictive models for asthma risk assessment

---

## References

Anselin, L. (1995). Local indicators of spatial association—LISA. *Geographical Analysis, 27*(2), 93-115.

Centers for Disease Control and Prevention. (2024). *PLACES: Local Data for Better Health, Census Tract Data 2024 release*. Retrieved from https://www.cdc.gov/places

Jerrett, M., Burnett, R. T., Ma, R., Pope III, C. A., Krewski, D., Newbold, K. B., ... & Thun, M. J. (2005). Spatial analysis of air pollution and mortality in Los Angeles. *Epidemiology, 16*(6), 727-736.

Mohai, P., & Saha, R. (2015). Which came first, people or pollution? Assessing the disparate siting and post-siting demographic change hypotheses of environmental injustice. *Environmental Research Letters, 10*(11), 115008.

U.S. Environmental Protection Agency. (2024). *Toxics Release Inventory (TRI) Basic Data Files*. Retrieved from https://www.epa.gov/toxics-release-inventory-tri-program

---

## License

This project is licensed under the MIT License.

---

## Acknowledgments

- CDC PLACES for providing tract-level health estimates
- U.S. Census Bureau for demographic and geographic data
- EPA for TRI facility data
- OpenStreetMap contributors for road and landuse data
