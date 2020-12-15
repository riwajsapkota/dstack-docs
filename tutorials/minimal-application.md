---
description: Here is an example of a very simple application.
---

# Minimal Application

The application takes real-time stock exchange data from Yahoo Finance for the FAANG companies and renders it. 

![](https://i.ibb.co/QCvkLBX/Screenshot-2020-11-27-at-17-00-57.png)

The user is prompted to choose one of the companies to view its latest market data in form of a candlestick chart.

```python
from datetime import datetime, timedelta

import dstack.controls as ctrl
import dstack as ds
import plotly.graph_objects as go
import pandas_datareader.data as web


def get_chart(symbols: ctrl.ComboBox):
    start = datetime.today() - timedelta(days=30)
    end = datetime.today()
    df = web.DataReader(symbols.value(), 'yahoo', start, end)
    fig = go.Figure(
        data=[go.Candlestick(x=df.index, open=df['Open'], high=df['High'], low=df['Low'], close=df['Close'])])
    return fig


app = ds.app(get_chart, symbols=ctrl.ComboBox(["FB", "AMZN", "AAPL", "NFLX", "GOOG"], require_apply=False))

result = ds.push("faang", app)
print(result.url)
```

Let's take a closer look at this code and describe every step.

**Application output**

First, we define the function `get_chart` that takes the argument `symbols` of the type `ctrl.ComboBox`. The argument represents a combo box in which the user selects a stock symbol \(e.g. `"FB"`, `"AMZN"`, etc\). Based on the selected symbol \(see `symbols.value()`\), the function fetches the market data for the corresponding stock \(from the Yahoo Financial Services – using the `pandas_datareader` package\), makes a Candlestick chart \(using the `plotly` package\), and returns the resulting figure.

```python
def get_chart(symbols: ctrl.ComboBox):
    start = datetime.today() - timedelta(days=30)
    end = datetime.today()
    df = web.DataReader(symbols.value(), 'yahoo', start, end)
    fig = go.Figure(
        data=[go.Candlestick(x=df.index, open=df['Open'], high=df['High'], low=df['Low'], close=df['Close'])])
    return fig
```

**User controls and application**

Once the function is defined, we call the function `dstack.app` where we pass our function that produces the output and assigns an instance of `ctrl.ComboBox` into the argument named `symbols`. This call creates an instance of an application. The application contains information on the function that produces the visualizations and binds an instance `ctrl.ComboBox` to the name of the argument of the function \(`symbols`\).

```python
app = ds.app(get_chart, symbols=ctrl.ComboBox(["FB", "AMZN", "AAPL", "NFLX", "GOOG"], require_apply=False))
```

**Deploy application**

Finally, we deploy our application to the `dstack` server by using the function `dstack.push`. The arguments of the call are `"faang"` – the name of the application, and `app` – the instance of our application. If successful, this call returns a push result that has an attribute `url`. This is the URL of the deployed application.

```python
result = ds.push("faang", app)
print(result.url)
```

If we click the URL, we'll see the application.

