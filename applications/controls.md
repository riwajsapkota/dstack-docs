---
description: Learn which type of controls are supported and how to user them.
---

# Controls

A user control is an element of the user interface that allows the user of the application to change input parameters. A `dstack` application may have any number of controls. Currently, `dstack` supports text fields, drop-downs, sliders, check-boxes. 

All controls must be passed as `**kwargs` to the function `dstack.app`. The arguments of the function, that produces the output of the application must exactly match the specified controls.

Here's a simple example:

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


app = ds.app(get_chart, symbols=ctrl.ComboBox(['FB', 'AMZN', 'AAPL', 'NFLX', 'GOOG']))

result = ds.push('faang', app)
print(result.url)
```

### Control API Reference

#### TextField

`dstack.controls.TextField`

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>data</code>
      </td>
      <td style="text-align:left">
        <p>It can be one of the following:</p>
        <ul>
          <li><code>str</code>
          </li>
          <li><code>Callable</code>
          </li>
        </ul>
      </td>
      <td style="text-align:left">
        <p>It can be one of the following:</p>
        <ul>
          <li>A plain text. <em>See example A.</em>
          </li>
          <li>A function that returns a plain text. <em>See example B.</em>
          </li>
          <li>A function that updates the state of the control. <em>See example C.</em>
          </li>
        </ul>
      </td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>long</code>
      </td>
      <td style="text-align:left"><code>bool</code>
      </td>
      <td style="text-align:left"><code>True</code> if the field may contain long values. <code>False</code> by
        default.</td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>label</code>
      </td>
      <td style="text-align:left"><code>str</code>
      </td>
      <td style="text-align:left">The caption of the text field.</td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>depends</code>
      </td>
      <td style="text-align:left">
        <p>It can be one of the following:</p>
        <ul>
          <li><code>List[dstack.controls.Control]</code>
          </li>
          <li><code>dstack.controls.Control</code>
          </li>
        </ul>
      </td>
      <td style="text-align:left">The list of other controls this control depends on. <em>See example D.</em>
      </td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>require_apply</code>
      </td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left"><code>True</code> if the field requires an <code>Apply</code> button to be
        clicked for the application to update the output. <code>True</code> by default.</td>
      <td
      style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>optional</code>
      </td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left"><code>True</code> if the filed&apos;s value is required for the application
        to provide the output. <code>False</code> by default.</td>
      <td style="text-align:left">No</td>
    </tr>
  </tbody>
</table>

#### ComboBox

`dstack.controls.ComboBox`

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>data</code>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>It can be one of the following:</p>
        <ul>
          <li><code>List[Any]</code>
          </li>
          <li><code>Callable</code>
          </li>
        </ul>
        <p></p>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>It can be one of the following:</p>
        <ul>
          <li>A list of items. <em>See example A.</em>
          </li>
          <li>A function that returns a list of items. <em>See example B.</em>
          </li>
          <li>A function that updates the state of the control. <em>See example C.</em>
          </li>
        </ul>
      </td>
      <td style="text-align:left">Yes</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>selected</code>
      </td>
      <td style="text-align:left">
        <p>It can be one of the following:</p>
        <ul>
          <li><code>int</code>
          </li>
          <li><code>List[int]</code>
          </li>
        </ul>
      </td>
      <td style="text-align:left">
        <p>It can be one of the following:</p>
        <ul>
          <li>An index of the currently selected item. Only if multiple set to False.</li>
          <li>A list of indexes of the currently selected items. Only if <code>multiple</code> set
            to <code>True</code>.</li>
        </ul>
      </td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>multiple</code>
      </td>
      <td style="text-align:left"><code>bool</code>
      </td>
      <td style="text-align:left"><code>True</code> if multiple selection is allowed. <code>False</code> by
        default.</td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>label</code>
      </td>
      <td style="text-align:left"><code>str</code>
      </td>
      <td style="text-align:left">The caption of the combo-box control.</td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>depends</code>
      </td>
      <td style="text-align:left">
        <p>It can be one of the following:</p>
        <ul>
          <li><code>List[dstack.controls.Control]</code>
          </li>
          <li><code>dstack.controls.Control</code>
          </li>
        </ul>
      </td>
      <td style="text-align:left">The list of other controls this control depends on. <em>See example D.</em>
      </td>
      <td style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>require_apply</code>
      </td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left"><code>True</code> if the field requires an <code>Apply</code> button to be
        clicked for the application to update the output. <code>True</code> by default.</td>
      <td
      style="text-align:left">No</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>optional</code>
      </td>
      <td style="text-align:left">bool</td>
      <td style="text-align:left"><code>True</code> if the filed&apos;s value is required for the application
        to provide the output. <code>False</code> by default.</td>
      <td style="text-align:left">No</td>
    </tr>
  </tbody>
</table>

#### CheckBox

`dstack.controls.CheckBox`

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>data</code>
      </td>
      <td style="text-align:left">
        <p>It can be one of the following:</p>
        <ul>
          <li><code>bool</code>
          </li>
          <li><code>Callable</code>
          </li>
        </ul>
      </td>
      <td style="text-align:left">
        <p>It can be one of the following:</p>
        <ul>
          <li>A <code>bool</code>. <em>See example A.</em>
          </li>
          <li>A function that returns a <code>bool</code>. <em>See example B.</em>
          </li>
          <li>A function that updates the state of the control. <em>See example C.</em>
          </li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

