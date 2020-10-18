---
description: >-
  You can use dstack to easily push and pull visualisations made with
  Matplotlib, Seaborn, Plotly, Bokeh.
---

# Using Visualisations with dstack.ai

## Matplotlib

Once the **dstack profile** is configured, you can publish plots from your Python program or Jupyter notebook. Let's consider the simpliest example, line plot using [matplotlib](https://matplotlib.org/) library, but you can use [bokeh](https://docs.bokeh.org/en/latest/index.html) and [plotly](https://plot.ly/) plots instead of matplotlib in the same way:

```python
import matplotlib.pyplot as plt
from dstack import push_frame

fig = plt.figure()
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])

push_frame("simple", fig, "My first plot")
```

## Plotly

dstack allows you to push `plotly.graph_objs._figure.Figure`

## Seaborn

```python
import seaborn as sns
sns.set_theme(style="darkgrid")

# Load an example dataset with long-form data
fmri = sns.load_dataset("fmri")

# Plot the responses for different events and regions
sns.lineplot(x="timepoint", y="signal",
             hue="region", style="event",
             data=fmri)
```

## Bokeh

dstack allows you to push `bokeh.plotting.figure.Figure`

## ggplot2 in R

#### Publishing simple plots in R

```r
library(ggplot2)
library(dstack)

df <- data.frame(x = c(1, 2, 3, 4), y = c(1, 4, 9, 16))
image <- ggplot(data = df, aes(x = x, y = y)) + geom_line()

push_frame("simple", image, "My first plot")
```

#### Publishing interactive plots in R

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

