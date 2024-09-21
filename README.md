# Currency Exchange Rate Forecasting using Time Series Analysis

This project focuses on forecasting the USD to INR exchange rate using time series analysis. The model uses SARIMA for predictions based on historical currency exchange data. The project also includes an analysis of seasonal trends and patterns in the exchange rate.

## Table of Contents
- [Dataset](#dataset)
- [Installation](#installation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Time Series Decomposition](#time-series-decomposition)
- [SARIMA Modeling](#sarima-modeling)
- [Forecasting](#forecasting)
- [Results Visualization](#results-visualization)
- [License](#license)

---

## Dataset
The dataset used in this project contains the historical USD to INR conversion rates.

Dataset source: **[INR-USD Historical Data](INR-USD.csv)**.

---

## Installation

1. Clone this repository:
    ```bash
    git clone https://github.com/mshaadk/Currency-Exchange-Rate-Forecasting.git
    ```

2. Install the required libraries:
    ```bash
    pip install -r requirements.txt
    ```

### Dependencies
- Python 3.7+
- pandas
- numpy
- matplotlib
- seaborn
- plotly
- statsmodels
- pmdarima

---

## Exploratory Data Analysis

### Handling Missing Data
```python
data.isnull().sum()
data = data.dropna()
```

### Descriptive Statistics
```python
data.describe()
```

### USD to INR Conversion Rate Trend
A line chart to visualize the exchange rate trend over the years:

```python
figure = px.line(data, x="Date", y="Close", title='USD - INR Conversion Rate over the years')
figure.show()
```

### Yearly Growth Visualization
Yearly growth of USD to INR conversion rate:

```python
fig = go.Figure()
fig.add_trace(go.Bar(x=growth.index, y=growth['Close'], name='Yearly Growth'))
fig.update_layout(title="Yearly Growth of USD - INR Conversion Rate", xaxis_title="Year", yaxis_title="Growth (%)", width=900, height=600)
pio.show(fig)
```

### Monthly Growth Visualization
Monthly average growth:

```python
fig = go.Figure()
fig.add_trace(go.Bar(x=grouped_data['Month'], y=grouped_data['Growth'], marker_color=grouped_data['Growth'], hovertemplate='Month: %{x}<br>Average Growth: %{y:.2f}%<extra></extra>'))
fig.update_layout(title="Aggregated Monthly Growth of USD - INR Conversion Rate", xaxis_title="Month", yaxis_title="Average Growth (%)", width=900, height=600)
pio.show(fig)
```

### Time Series Decomposition
The seasonal decomposition of the exchange rate data to identify patterns:

```python
from statsmodels.tsa.seasonal import seasonal_decompose
result = seasonal_decompose(data["Close"], model='multiplicative', period=24)
result.plot().show()
```

## SARIMA Modeling
Using SARIMA to forecast exchange rates:

### Parameter Estimation
```python
from pmdarima.arima import auto_arima
model = auto_arima(data['Close'], seasonal=True, m=52, suppress_warnings=True)
print(model.order)  # Output: (2, 1, 0)
```

### Model Training
```python
from statsmodels.tsa.statespace.sarimax import SARIMAX
model = SARIMAX(data["Close"], order=(2, 1, 0), seasonal_order=(2, 1, 0, 52))
fitted = model.fit()
print(fitted.summary())
```

### Forecasting
We forecast the future currency exchange rates:

```python
predictions = fitted.predict(len(data), len(data)+60)
print(predictions)
```

### Results Visualization
A comparison between the training data and predictions:

```python
fig = go.Figure()
fig.add_trace(go.Scatter(x=data.index, y=data['Close'], mode='lines', name='Training Data', line=dict(color='blue')))
fig.add_trace(go.Scatter(x=predictions.index, y=predictions, mode='lines', name='Predictions', line=dict(color='green')))
fig.update_layout(title="INR Rate - Training Data and Predictions", xaxis_title="Date", yaxis_title="Close", width=900, height=600)
pio.show(fig)
```

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE.txt) file for details.
