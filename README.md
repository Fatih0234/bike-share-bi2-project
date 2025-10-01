# Cyclistic Bike Share Analysis Project

A comprehensive data science project analyzing bike-sharing patterns to distinguish between casual riders and annual members, providing strategic insights for converting casual riders into annual memberships.

## 📊 Project Overview

This project analyzes 12 months of Divvy bike share data from Chicago (April 2021 - March 2022) to understand usage patterns between casual riders and annual members. The analysis incorporates weather data and uses machine learning to identify key differentiators between user types.

**Key Business Question**: How do annual members and casual riders use Cyclistic bikes differently?

## 🎯 Objectives

- Analyze temporal patterns (daily, weekly, seasonal) of bike usage
- Identify differences in ride duration and destination preferences  
- Incorporate weather impact on riding behavior
- Build predictive models to classify rider types
- Provide data-driven recommendations for membership conversion strategies

## 📁 Project Structure

```
cyclist_bike_share/
├── 1-vibing.ipynb              # Main analysis notebook
├── EDA/                        # Exploratory Data Analysis outputs
│   ├── bike_heatmap.html       # Interactive heatmap of ride locations
│   ├── hourly_ride_share_weekends.png
│   ├── monthly_share.png
│   ├── ride_duration.png
│   └── weekday_vs_weekend_share.png
├── Preprocessing/              # Data cleaning visualizations
│   ├── missing_values_heatmap.png
│   └── outlier_detection.png
├── Modelling/                  # Machine learning results
│   ├── logreg_features.png
│   ├── logreg_results.png
│   ├── rf_features.png
│   ├── rf_results.png
│   ├── xgb_features.png
│   └── xgb_results.png
├── weather_cache_openmeteo/    # External weather data cache
└── README.md
```

## 🔍 Key Findings

### 1. **Temporal Usage Patterns**
- **Members**: Consistent weekday usage with peaks at 7-9 AM and 4-6 PM (commuter pattern)
- **Casuals**: Higher weekend usage, more leisure-oriented timing
- **Seasonality**: Casual usage highly elastic to weather (17% peak in July vs 2% in winter), while members maintain more consistent year-round usage

### 2. **Ride Duration Differences** 
- **Members**: Short, efficient trips (median ~10-12 minutes)
- **Casuals**: Longer leisure rides, more variable duration
- **Round Trips**: Casuals frequently take round trips from tourist hubs (Navy Pier, Millennium Park), while members use point-to-point commuting

### 3. **Weather Impact**
- Temperature, precipitation, and wind significantly affect casual ridership
- Members show more weather resilience, consistent with commute necessity
- Seasonal variations more pronounced for casual users

### 4. **Geographic Patterns**
- Tourist destinations and recreational areas popular with casuals
- Business districts and residential areas favored by members
- Interactive heatmap reveals distinct usage clusters

## 🛠️ Technical Implementation

### Data Processing
- **Dataset**: 12 months of Divvy trip data (5.7M+ records)
- **External Data**: Weather data integrated from Open-Meteo API
- **Data Cleaning**: Removed negative/zero durations, geographic outliers, and rides >24 hours
- **Feature Engineering**: Created temporal features (hour, day, season), weather bins, and duration categories

### Machine Learning Models

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|-------|----------|-----------|--------|----------|---------|
| **Logistic Regression** | 0.892 | 0.917 | 0.944 | 0.930 | 0.954 |
| **Random Forest** | 0.933 | 0.945 | 0.962 | 0.953 | 0.976 |
| **XGBoost** | 0.939 | 0.949 | 0.967 | 0.958 | 0.979 |

### Key Predictive Features
1. **Rush Hour Usage** - Strong predictor of membership
2. **Ride Duration** - Short trips indicate members
3. **Day of Week** - Weekday patterns favor members
4. **Weather Conditions** - Temperature and precipitation impact
5. **Start Hour** - Morning/evening commute patterns

## 📈 Business Recommendations

### 1. **Targeted Marketing Campaigns**
- **Summer Focus**: Leverage casual riders' peak summer usage (June-August)
- **Weekend Promotions**: Target leisure riders with weekend membership trials
- **Weather-Based**: Dynamic pricing/promotions during favorable weather

### 2. **Infrastructure Optimization**
- **Tourist Areas**: Increase bike availability at popular casual destinations
- **Commuter Routes**: Ensure reliable service during rush hours for members
- **Seasonal Adjustments**: Redistribute bikes based on seasonal patterns

### 3. **Membership Conversion Strategies**
- **Trial Memberships**: Offer short-term memberships during peak casual usage periods
- **Commuter Incentives**: Highlight time and cost savings for regular commuters
- **Weather Resilience**: Market membership benefits during weather transitions

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost folium requests
```

### Running the Analysis
1. Clone the repository
2. Install required packages
3. Open `1-vibing.ipynb` in Jupyter Notebook/VS Code
4. Run cells sequentially (dataset not included due to size)

### Data Sources
- **Divvy Trip Data**: [Chicago Data Portal](https://divvy-tripdata.s3.amazonaws.com/index.html)
- **Weather Data**: [Open-Meteo Archive API](https://open-meteo.com/)

## 📊 Visualization Highlights

- **Interactive Heatmap**: Geographic distribution of ride start points
- **Temporal Analysis**: Hourly, daily, and seasonal usage patterns
- **Duration Analysis**: Distribution differences between user types
- **Feature Importance**: Machine learning model insights
- **Weather Impact**: Correlation analysis with ridership patterns

## 🔧 Technical Stack

- **Python**: Primary programming language
- **Pandas/NumPy**: Data manipulation and analysis
- **Scikit-learn**: Machine learning models and preprocessing
- **XGBoost**: Advanced gradient boosting
- **Matplotlib/Seaborn**: Statistical visualizations
- **Folium**: Interactive maps and heatmaps
- **Jupyter Notebook**: Interactive development environment

## 📝 Future Enhancements

- Real-time prediction API for rider classification
- Advanced clustering analysis for station optimization
- Integration with additional data sources (events, traffic)
- Deep learning models for temporal sequence prediction
- A/B testing framework for conversion strategies

## 🤝 Contributing

This project was developed as part of a comprehensive data science analysis. Contributions and suggestions are welcome for extending the analysis or improving methodologies.

## 📜 License

This project is for educational and analytical purposes. Data sources maintain their respective licensing terms.

---

**Author**: Data Science Analysis Project  
**Last Updated**: October 2025  
**Contact**: [GitHub Repository](https://github.com/Fatih0234/bike-share-bi2-project)
