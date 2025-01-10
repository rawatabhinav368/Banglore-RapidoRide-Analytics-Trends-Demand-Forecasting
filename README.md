# Banglore-RapidoRide-Analytics-Trends-Demand-Forecasting

## Table of Contents

1. [Project Overview](#project-overview)
2. [Data Sources](#data-sources)
3. [Tools](#tools)
4. [Data Cleaning/Preparation](#data-cleaningpreparation)
5. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
6. [Data Analysis](#data-analysis)
7. [Results/Findings](#resultsfindings)
8. [Recommendations](#recommendations)


### Project Overview

The project aims to analyze and forecast ride-sharing trends for Rapido in Bangalore, focusing on understanding user behavior, predicting ride demand, and deriving actionable business insights to improve operational efficiency.

### Data Sources 

**Dataset Source:**

The primary dataset used for this analysis is the **"rides_data.csv"** [Download rides_data.csv](https://github.com/rawatabhinav368/Hospital-Supply-Chain-Inventory-Optimization/blob/main/inventory_data.csv) file, which was downloaded from Kaggle's [Bangalore Rapido Ride Services Dataset](https://www.kaggle.com/datasets/vishaldeoprasad/bangalore-rapido-ride-services-dataset?select=rides_data.csv).  This dataset contains detailed information about Bangalore Rapido Ride Services, including the following features:

- * ### Date - year-month-day
- * ### Time - Hour-Minute (HH:MM)
- * ### Ride Status - Indicates whether the ride was completed or canceled.
- * ### Service - Type of service offered (e.g., Bike, Car, etc.).
- * ### Source- The origin or pickup location of the ride 
- * ### Destination - The destination or drop-off point for the ride
- * ### Duration - The duration of the ride in minutes.
- * ### Ride id - A unique identifier assigned to each ride.
- * ### Distance- The distance traveled during the ride, measured in kilometers.
- * ### Ride charge- The base fare for the ride, excluding any additional charges, in local currency.
- * ### Misc charge - Additional charges for the ride (e.g., taxes, service fees), in local currency.
- * ### Total fare - The total fare for the ride, including all charges (Ride charge + Misc charge).
- * ### payment method - The payment method used for the ride.

For more information, visit the Kaggle dataset page [here](https://www.kaggle.com/datasets/vishaldeoprasad/bangalore-rapido-ride-services-dataset?select=rides_data.csv).


### Tools

1. **Python**: 
    Libraries Used: - Pandas , Numpy, Matplotlib, Seaborn, Plotly, Warnings.
  
2. **Power Bi**: 
   - Power BI was used for data visualization. It helped in creating interactive dashboards to analyze Bangalore Rapido Ride Services  data, service demand, and identify trends in real-time.
   - ## RapidoRide Trends: Bangalore Analytics & Demand Forecasting 

Click the image below to view the dashboard:

[![Rapido Banglore Service and Demand Anlytics Dashboard](https://github.com/rawatabhinav368/Banglore-RapidoRide-Analytics-Trends-Demand-Forecasting/blob/main/RapidoRide%20Trend%20Bangalore%20Analytics%20And%20Demand%20Forecasting.png)](https://github.com/rawatabhinav368/Banglore-RapidoRide-Analytics-Trends-Demand-Forecasting/blob/main/RapidoRide%20Trend%20Bangalore%20Analytics%20And%20Demand%20Forecasting.png)

3. **PowerPoint**:
   - PowerPoint was used for creating presentations and communicating the insights derived from the analysis. It helped in effectively presenting findings and recommendations to stakeholders.
   - ## PowerPoint Presentation

You can download and view the PowerPoint presentation for this analysis using the link below:

[Banglore-RapidoRide-Analytics-Trends-Demand-Forecasting](https://github.com/rawatabhinav368/Banglore-RapidoRide-Analytics-Trends-Demand-Forecasting/blob/main/Rapido%20banglore%20service%20and%20demand%20analytics%20pdf.pdf)

  ### Data Cleaning/Preparation

  In the initital data preparation pharase, we preformed the following tasks:
  
  1. Data Loading and inspection 
  
  2. Checking for missing values -

* There was 5036 null values in our data set, for all cancelled ride (ride_charge	misc_charge	total_fare	payment_method) it shows NaN. 
  A canceled ride does not incur any charges, so filling 0.
* No transaction occurred, "Not Applicable" is a meaningful label for payment method.
* Created canceled column : 1 for canceled rides, 0 for all other rides.

  ### Exploratory data analysis (EDA)
 
  EDA involves exploring the ride data to answer key questions, such as:

  - Which service type (e.g., Bike, Auto, Cab) is most preferred by users?
  - What are the peak demand hours for each service type?
  - How do service preferences change throughout the day or week?
  - What proportion of rides are completed versus canceled?
  - Which payment methods are most popular among users?
  - How strongly are features like distance, duration, and fare related?
  - Are there specific periods that contribute significantly to revenue spikes?
  - 

### Data Analysis 

Including some intresting code/features worked with 

```python
# Distribution of numerical columns (like duration, distance, ride_charge, etc.)
sns.set(style="whitegrid")
numerical_cols = ['duration', 'distance', 'ride_charge', 'misc_charge', 'total_fare']
rapido_data[numerical_cols].hist(bins=20, figsize=(12, 10))
plt.tight_layout()
plt.show()

SARIMAX Forecast for ride demand 
sarimax_model = SARIMAX(endog, 
                        order=(1, 1, 1),  # ARIMA order
                        seasonal_order=(1, 1, 1, 7))  # Weekly seasonality (7 days)
sarimax_results = sarimax_model.fit()

# Forecast for the next 30 days
forecast_steps = 30
sarimax_forecast = sarimax_results.get_forecast(steps=forecast_steps)
sarimax_forecast_values = sarimax_forecast.predicted_mean
sarimax_forecast_index = pd.date_range(start=daily_demand['date'].iloc[-1] + pd.Timedelta(days=1), 
                                       periods=forecast_steps, freq='D')
# SARIMAX Forecast
fig.add_trace(go.Scatter(x=sarimax_forecast_index, y=sarimax_forecast_values,
                         mode='lines+markers', name='SARIMAX Forecast', line=dict(color='orange')))

ARIMA Forecast for ride demand
from statsmodels.tsa.arima.model import ARIMA
import plotly.graph_objects as go

arima_model = ARIMA(daily_demand['ride_count'], order=(1, 1, 1))
arima_results = arima_model.fit()

# Forecast for the next 30 days
arima_forecast = arima_results.forecast(steps=forecast_steps)
arima_forecast_index = pd.date_range(start=daily_demand['date'].iloc[-1] + pd.Timedelta(days=1), 
                                     periods=forecast_steps, freq='D')
fig.add_trace(go.Scatter(x=arima_forecast_index, y=arima_forecast,
                         mode='lines+markers', name='ARIMA Forecast', line=dict(color='red')))
# Updating Layout
fig.update_layout(title="Daily Ride Demand Forecast for the Next 30 Days",
                  xaxis_title="Date",
                  yaxis_title="Ride Count",
                  showlegend=True)

fig.show()

```

### Results/Findings

The analysis results are summarized as follows:
* High demand for bike services during early mornings, auto and cab demand peaks in the afternoon and evening.
* Ride counts fluctuate over time, with consistent peaks during specific periods.
* Paytm and GPay are the most popular payment methods, possibly influenced by promotions.
* Bike services are the most preferred, offering consistent ride durations, while auto and cab services show more variability.
*  ARIMA and SARIMAX models suggest growing demand with daily fluctuations.

  ## Recommendations
* Increase bike availability during morning hours and prioritize auto/cab services in the evening.
* Promote bike services as a cost-effective, quick option for short-distance travel.
* For auto and cab services, focus on improving ride consistency by reducing variability in ride durations (e.g., through better route optimization or partnerships with local authorities to reduce traffic bottlenecks).
*  Capitalize on Paytm and GPay’s popularity by offering time-limited discounts to further drive engagement.
*  Promote premium cab services during evenings for social outings or corporate travel to attract higher-paying customers.


