# Winter Wheat BBCH Prediction Model

A machine learning model to predict phenological development stages (BBCH) of winter wheat using Growing Degree Days (GDD) calculated from German Weather Service (DWD) climate data.

## Overview

This project builds a predictive model for winter wheat (*Triticum aestivum*) phenology using:
- **DWD Phenology Data**: Historical BBCH stage observations from ~5,800 German stations (1925-2023)
- **DWD Climate Data**: Daily temperature records for calculating thermal time accumulation

## BBCH Stages

The BBCH scale is a standardized system for encoding phenological growth stages:

| BBCH | Stage |
|------|-------|
| 10 | Emergence |
| 12 | Two leaves unfolded |
| 21 | Beginning of tillering |
| 31 | Beginning of stem elongation |
| 41 | Flag leaf sheath extending |
| 51 | Beginning of heading |
| 61 | Beginning of flowering |
| 75 | Medium milk ripening |
| 87 | Hard dough |

## Growing Degree Days (GDD)

The model uses accumulated thermal time (Growing Degree Days) as the primary predictor:

```
GDD = Σ max(0, T_mean - T_base)
```

Parameters:
- **Base temperature (T_base)**: 0°C
- **Upper temperature cutoff**: 30°C

## Setup

1. Create and activate virtual environment:
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

2. Install dependencies:
```bash
pip install numpy pandas matplotlib seaborn scikit-learn requests scipy jupyter
```

3. Run the notebook:
```bash
jupyter notebook notebook.ipynb
```

## Project Structure

```
ML-project/
├── notebook.ipynb          # Main Jupyter notebook
├── data/                   # Cached DWD data (auto-downloaded)
│   ├── phenology_winterwheat.csv
│   └── climate/
│       └── climate_station_*.csv
├── bbch_prediction_model.pkl  # Trained model (after running notebook)
├── venv/                   # Virtual environment
└── README.md
```

## Data Sources

Data is automatically downloaded and cached from the DWD Open Data server:
- Phenology: https://opendata.dwd.de/climate_environment/CDC/observations_germany/phenology/
- Climate: https://opendata.dwd.de/climate_environment/CDC/observations_germany/climate/daily/kl/

## Models

The notebook trains and compares multiple regression models:
- Linear Regression
- Random Forest
- Gradient Boosting
- K-Nearest Neighbors

## Output

After running the notebook:
- Model comparison visualizations (`model_comparison.png`)
- GDD thresholds for each BBCH stage
- Trained model saved as `bbch_prediction_model.pkl`

## Usage

Load and use the trained model:

```python
import pickle

with open('bbch_prediction_model.pkl', 'rb') as f:
    model_data = pickle.load(f)

model = model_data['model']
gdd = 500  # Accumulated Growing Degree Days
predicted_bbch = model.predict([[gdd]])[0]
```

## License

Data provided by DWD Climate Data Center (CDC) under their terms of use.
