---
description: Learn which types of outputs are supported and how to generate them.
---

# Outputs

The output of the application is defined by the function passed to `dstack.app()`. See the function `get_chart` in the following example: 

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


app = ds.app(get_chart, symbols=ctrl.ComboBox(["FB", "AMZN", "AAPL", "NFLX", "GOOG"]))

result = ds.push("faang", app)
print(result.url)
```

Within such a function, it's allowed to return a single object of the type that is supported. The supported base types include: 

* `pandas.core.generic.NDFrame` \(a Pandas dataframe\)
* `plotly.graph_objs._figure.Figure` \(a Plotly figure\)
* `matplotlib.figure.Figure` \(a Matplotlib figure\)
* `bokeh.plotting.Figure` \(a Bokeh figure\)

Given the supported base types, it's possible to use `Pandas`, `Plotly`, `Bokeh`, `Matplotlib`, and `Seaborn` for the output of the application. If you'd like to use something else as an output, please file an issue to the [tracker](https://github.com/dstackai/dstack/issues/).

{% hint style="warning" %}
Note, currently, it's not allowed to return multiple outputs. Please upvote the [corresponding issue](https://github.com/dstackai/dstack/issues/39) in the tracker.
{% endhint %}

