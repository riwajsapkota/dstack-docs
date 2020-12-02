---
description: >-
  You can use dstack to easily push and pull visualisations made with
  Matplotlib, Seaborn, Plotly, Bokeh.
---

# Using Visualisations

## Matplotlib

Once the **dstack profile** is configured, you can publish plots from your Python program or Jupyter notebook. Let's consider the simpliest example, line plot using [matplotlib](https://matplotlib.org/) library, but you can use [bokeh](https://docs.bokeh.org/en/latest/index.html) and [plotly](https://plot.ly/) plots instead of matplotlib in the same way:

### Simple Plot

```python
import matplotlib.pyplot as plt
import dstack as ds

fig = plt.figure()
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])

ds.push("simple", fig, "My first plot")
```

### Interactive Plot

```python
import matplotlib.pyplot as plt
import dstack as ds

def line_plot(a):
    xs = range(0, 21)
    ys = [a * x for x in xs]
    fig = plt.figure()
    plt.axis([0, 20, 0, 20])
    plt.plot(xs, ys)
    return fig


frame = ds.frame("line_plot")
coeff = [0.5, 1.0, 1.5, 2.0]

for c in coeff:
    frame.commit(line_plot(c), f"Line plot with the coefficient of {c}", Coefficient=c)

frame.push("Adding message to my push for interactive plot")
```

## Plotly

The object type `plotly.graph_objs._figure.Figure` for Plotly plots is support by dstack.

```python
import plotly.express as px
import dstack as ds


df = px.data.gapminder().query("country=='Canada'")
fig = px.line(df, x="year", y="lifeExp", title='Life expectancy in Canada')

ds.push("plotly_plot", fig, "My plotly plot")
```

## Bokeh

dstack allows you to push `bokeh.plotting.figure.Figure`

```python
from bokeh.plotting import figure, output_file, show
import dstack as ds

# prepare some data
x = [1, 2, 3, 4, 5]
y = [6, 7, 2, 4, 5]

# create a new plot with a title and axis labels
p = figure(title="simple line example", x_axis_label='x', y_axis_label='y')

# add a line renderer with legend and line thickness
p.line(x, y, legend_label="Temp.", line_width=2)

# show the results
ds.push("bokeh_plot", p, "My bokeh plot")
```

## ggplot2 in R

### Publishing simple plots in R

```r
library(ggplot2)
library(dstack)

df <- data.frame(x = c(1, 2, 3, 4), y = c(1, 4, 9, 16))
image <- ggplot(data = df, aes(x = x, y = y)) + geom_line()

push("simple", image, "My first plot")
```

### Publishing interactive plots in R

Suppose you want to publish a line plot that depends on the value of the parameter `Coefficient`\(slope\).

```r
library(ggplot2)
library(dstack)

line_plot <- function(a) { 
    x <- c(0:20)
    y <- sapply(x, function(x) { return(a * x) })
    df <- data.frame(x = x, y = y)
    plot <- ggplot(data = df, aes(x = x, y = y)) + 
        geom_line() + xlim(0, 20) + ylim(0, 20)
    return(plot)
}

coeff <- c(0.5, 1.0, 1.5, 2.0)
frame <- create_frame(stack = "line_plot")
for(c in coeff) {  
    frame <- commit(frame, line_plot(c), 
        paste0("Line plot with the coefficient of ", c), list(Coefficient = a))
}

push(frame)
```

