# Time Series Analysis of Vegetable and Fruit Prices
(Veggie) Market Forecasting

## Table of Contents

- [About the Dataset](#about-the-dataset)
- [Source](#source)
- [Executive Summary](executive-summary)
- [Commodities Explored](#commodities-explored)
- [Red Potato](#red-potato)
- [Broad Leaf Mustard](#broad-leaf-mustard)
- [Okra](#okra)
- [Conclusion](#conclusion)
- [Lesson Learned](#lesson-learned)

### About the Dataset

- .csv format time series
- Official price information for major vegetables and fruits in Nepal from 2013 to 2021
- This time series dataset can be used for forecasting future prices of vegetables and fruits in 
Nepal, analyzing price trends and seasonality and identifying outliers and irregularities. Through these strategies, we can develop pricing strategies for the use of farmers and market vendors.

![data](https://github.com/Venu-Jakkula/Time-Series-Analysis-of-Vegetable-and-Fruit-Prices/assets/171456105/d21805c3-ccef-40cb-a144-4e41f76273ec)

### Source

- The Ministry of Agriculture and Livestock Development publishes daily price information for major vegetables and fruits and lists this information on the website of the Government of Nepal. This is the source of this information.
- From the website, the data was collected by Nischal Lal Shrestha, and listed on Kaggle Datasets [Time Series Price Vegetables and Fruits](https://www.kaggle.com/datasets/ramkrijal/agriculture-vegetables-fruits-time-series-prices)

### Executive Summary

#### Process:

- A Time Series Exploration of all commodities included in the dataset was conducted. This document is included in the submitted documents (initial_Time Series Exploration-results.pdf). This included a baseline analysis for each of the 132 commodities included in the dataset. Based on this initial exploration, 13 commodities were identified for further exploration. Commodities were chosen based on the availability of data (i.e., excluding those with large amounts of missing data), seasonality, trend, and complexity. Commodities that showed interesting behavior that might provide valuable insights were also selected.

- Data preparation involved two steps:
  - A) Coding with Python in Google Collaboratory to create subsets of the raw dataset

  ```python
  import pandas as pd
  """
  Replace 'your_file.csv' with the path to your CSV file
  """
  csv_file_path = '/content/drive/MyDrive/kalimati_tarkari_dataset.csv'
  """
  Use the pandas 'read_csv' function to read the CSV file into a DataFrame
  """
  df = pd.read_csv(csv_file_path)
  """
  Now, you can work with the DataFrame 'df' to analyze or manipulate your data
  For example, you can display the first few rows of the DataFrame using 'head()':
  """
  print(df["Commodity"].unique())
  print(df["Commodity"].nunique())
  ```
  
   ```python
  subset_Potato_Red = df[df['Commodity'] == 'Potato Red']
  print(subset_Potato_Red.head())
   ```

  - B) Transforming the Day interval time series data into month interval, using the Time Series Data Preparation tool in SAS. Average accumulation was used for this transformation.
    ![pic](https://github.com/Venu-Jakkula/Time-Series-Analysis-of-Vegetable-and-Fruit-Prices/assets/171456105/20db926e-8eb7-4102-95e2-0816877d591a)

 - After data preparation and more in-depth analysis, the 13 previously selected commodities were narrowed down to 3 key commodities that were the most interesting to explore.

  For each of the commodities, the following general steps were followed to build the models:
- A) Run a Time Series Exploration with the Time Series Variable = Average.
- B) Analyze the trend and seasonality components.
- C) Identify candidate exponential smoothing models based on trend and seasonality.
- D) Run candidate exponential smoothing models with the Number of periods to holdback = 12. Compare accuracy statistics from the Forecast (holdout sample). See additional notes below for the Red Potato commodity.
- E) If ESM is insufficient for accurate statistics and residuals, check the Unit Root Test.
- F) If the time series is stationary, run ARMA models. Use macros2.sas for accuracy measurements.

### Commodities Explored
- Red Potato
- Broad Leaf Mustard
- Okra

### Red Potato

- Best model: AR(5) model
- Reasons for selecting this model:
   - Winters additive exponential smoothing model had the best RMSE, MAPE, and MAE. It had similar AIC and SBC to other exponential smoothing models.
   - However, residuals in the White Noise Test indicated unexplained signal in the model.
   - Residuals are much better with the AR(5) model. Sacrifice model simplicity (parsimoniousness) to achieve less bias. RMSE, MAPE, and MAE are similar to other models.
   - ARMA model is appropriate given the lack of White Noise.
- Other models tried:
   - Winters additive exponential smoothing model
   - Winters multiplicative exponential smoothing model
   - Additive seasonal exponential smoothing model
   - Multiplicative seasonal exponential smoothing model
   - AR(1) model
   - AR(2) model
- Model accuracy statistics:
  ![pict](https://github.com/Venu-Jakkula/Time-Series-Analysis-of-Vegetable-and-Fruit-Prices/assets/171456105/57e12ecf-8763-4ba1-941b-960ef196dd92)

### Broad Leaf Mustard

- Best model: Additive seasonal exponential smoothing model
- Reasons for selecting this model:
  - Best overall combination of RMSE, MAPE, AIC, and SBC
- Model accuracy statistics:

  ![Pict1](https://github.com/Venu-Jakkula/Time-Series-Analysis-of-Vegetable-and-Fruit-Prices/assets/171456105/6cca4019-934d-42ed-88c9-ddb91a032729)

### Okra

- Best model: Additive seasonal exponential smoothing - Logistic
- Reasons for selecting this model: 
  - Best overall combination of RMSE, MAPE, AIC, and SBC.
  - Compared to other models, this model has relatively less white noise.
- Model accuracy statistics:

  ![Pict2](https://github.com/Venu-Jakkula/Time-Series-Analysis-of-Vegetable-and-Fruit-Prices/assets/171456105/081e11df-ed1a-4301-98bb-e1b154b9beca)

### Conclusion

- All three commodities (Broad Leaf Mustard, Okra, and Red Potato) experience seasonal price changes. This is a likely and expected phenomenon with agricultural products.
- There is a long-term, positive trend, most notably with Red Potato. The Broad Leaf Mustard and Okra are more consistent across time.
- Based on seasonality and historical trends, we can forecast future prices! This is helpful for farmers to prepare for the approaching seasons.

### Lesson Learned

- Data Accumulation: Using Daily time ID interval introduced too much noise into the model. Needed to use a weekly, monthly, or quarterly interval instead.
- Not all commodities work with the same type of model. While some commodities worked better with Exponential Smoothing models, others worked better with ARMA models.
- It is important to consider unique problems with the dataset. For example, holdout periods can not be blindly applied to all datasets, as they may result in losing important information.












