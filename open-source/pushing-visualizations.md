# Stacks

## What are Stacks?

Stacks are the heart and soul of dstack.ai. A stack in the English language is a pile of things - typically one that is neatly arranged. With dstack.ai, you can create stacks of **datasets, visualizations**, and **models** to create and save different versions of them, easily share them to collaborate with your team, re-use them as you see fit, and also create [Dashboards](dashboards.md) with them that help you tell a story with multiple stacks.

The `dstack` tool for Python and R lets you push datasets, models and visualizations, and arrange them into reports and interactive dashboards. You can read more detailed tutorials and examples about how to do this in the [Tutorials](../tutorials/dashboards-tutorial.md) Section.

{% hint style="info" %}
Uploading datasets and visualization to dstack.ai is done via the `dstack` package available for both Python and R. These packages can be used from Jupyter notebooks, RMarkdown, Python R scripts, and applications. Learn how to [install and configure](installation.md) `dstack`.
{% endhint %}

## Pushing static visualizations or datasets

Here's a simple example of the code that pushes a data visualization to dstack.ai:

{% tabs %}
{% tab title="Python" %}
```python
import matplotlib.pyplot as plt
import dstack as ds

fig = plt.figure()
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])

ds.push("simple", fig, "My first plot")
```
{% endtab %}

{% tab title="R" %}
```r
library(ggplot2)
library(dstack)

df <- data.frame(x = c(1, 2, 3, 4), y = c(1, 4, 9, 16))
image <- ggplot(data = df, aes(x = x, y = y)) + geom_line()

push_frame("simple", image, "My first plot")
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The `dstack` package can be used with [pandas](https://pandas.pydata.org/), [tidyverse](https://www.tidyverse.org/), [matplotlib](https://matplotlib.org/), [ggplot2](https://ggplot2.tidyverse.org/), [bokeh](https://docs.bokeh.org/en/latest/index.html) and [plotly](https://plot.ly/). The `commit` and `push_frame` methods accept `pandas.core.frame.DataFrame`, `data.frame`, `data.table`, `tibble`, `plotly.graph_objs._figure.Figure`, `bokeh.plotting.figure.Figure`, etc.
{% endhint %}

You can read more detailed and library-specific tutorials on pushing [datasets](../tutorials/datasets.md) and [visualizations](../tutorials/plotting-libraries.md) under the Tutorials and Guides tab.

## Pushing interactive visualizations and datasets

In some cases, you want to have plots that are interactive and that can change when the user change its parameters. Suppose you want to publish a line plot that depends on the value of the parameter`Coefficient`:

{% tabs %}
{% tab title="Python" %}
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
    frame.add(line_plot(c), 
    f"Line plot with the coefficient of {c}", {"Coefficient": c})

frame.push()
```
{% endtab %}

{% tab title="R" %}
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
        paste0("Line plot with the coefficient of ", c), 
        Coefficient = c)
}

push(frame)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
You can have as many parameters as you want. Within one frame, you can combine different types of data: dataframes and figures. 
{% endhint %}

dstack.ai tracks every submitted frame and lets you rollback between revisions via the web application if needed.

{% hint style="warning" %}
All visualizations and datasets pushed to dstack.ai follow the privacy settings specified for the registered profile. You can make all data submitted to dstack.ai either public or private. You also can change the privacy settings for individual datasets and visualizations to make them either public or private, or share them only with selected users. [Learn more on sharing and collaboration](../in-cloud/collaboration.md)
{% endhint %}

**That is it. Once the data is pushed to dstack.ai, you can share it with the team or arrange into** [**comprehensive dashboards**](dashboards.md) **📈.**

{% page-ref page="dashboards.md" %}

## Using Markdown and LaTeX

Sometimes you need to have more than just text in frame description. Fortunately, _dstack_ supports Markdown with embedded LaTeX formulas. There is an example how to use them all together:

{% tabs %}
{% tab title="Python" %}
```python
import dstack as ds
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-3, 3, 100)
y = np.exp(x)
fig = plt.figure()
plt.plot(x, y)


text = """
# Using markdown

* item1
* item2
* item3

Here is an example of $\LaTeX$ math:

$$e^x=\sum_{i=0}^\infty \\frac{1}{i!}x^i$$

It's sometimes very useful.
"""

ds.push("exponent", fig, description=text)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Be sure that you escape all special symbols, e.g. `\f` in `\frac`. Certainly, it is possible to escape all  to be safe.
{% endhint %}

