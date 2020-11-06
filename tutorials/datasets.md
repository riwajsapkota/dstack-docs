# Using Datasets

Effective collaboration on datasets is the key ingredient of any data science project. dstack.ai offers APIs and services to upload datasets, track their revisions and to share these datasets securely within teams \(or publicly if needed\).

The dstack package for datasets is currently compatible with `pandas.core.frame.DataFrame`, `data.frame`, `data.table`, and `tibble`.

{% hint style="info" %}
Make sure you have the `dstack` package installed \(available both in Python and R\) before you try to upload datasets and visualisations. These packages can be used from Jupyter notebooks, RMarkdown, Python and R scripts and applications. [Learn how to install dstack package](../in-cloud/installation.md)
{% endhint %}

## Pushing single datasets

Here's an example of the code that pushes a single dataset to dstack.ai:

{% tabs %}
{% tab title="Python" %}
```python
import pandas as pd
import numpy as np
import dstack as ds

dates = pd.date_range('20130101', periods=6)
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))

ds.push("static_dataset_example", df, "static dataset")
```
{% endtab %}

{% tab title="R" %}
```r
library(ggplot2)
library(dstack)

data("midwest", package = "ggplot2")
push_frame("simple", midwest, "My first dataset")
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The API supports datasets of any size. You can work with small datasets as well as with very large datasets.
{% endhint %}

## Pushing multiple datasets <a id="pushing-interactive-visualizations-and-datasets"></a>

In some cases, you want to push multiple datasets at once and associate each with particular parameter. Suppose you'd to publish multiple datasets on players for every parameter`College`:

{% tabs %}
{% tab title="Python" %}
```python
import pandas as pd
import dstack as ds

df = pd.read_csv("player_data.csv").dropna()
frame = ds.frame("player_data")

pdf = df['college'].value_counts().rename_axis('college').reset_index(name='players').head(10)
frame.add(pdf, f"Top 10 colleges by number of players", { "College": "Top 10 colleges" })

for college in df["college"].unique():
    players = df.loc[df["college"] == college]
    frame.add(players, f"Players from {college}", { "College": college })

frame.push()
```
{% endtab %}
{% endtabs %}

Once the dataset is pushed to dstack.ai, it can be accessed by the URL specified in the frame and the username configured with the `dstack` package: `https://<username>/<stackname>`

{% hint style="warning" %}
All datasets pushed to dstack.ai follow the privacy settings specified for the registered profile. You can make all data submitted to dstack.ai either public or private. You also can change the privacy settings for individual datasets to either public or private, or share them only with selected users. [Learn more on sharing and collaboration](../in-cloud/collaboration.md)
{% endhint %}

## Pulling datasets

Imagine a scenario that you would like to use a dataset published earlier or from someone else. In this case, you have two options to obtain the dataset to use it:

1. Download it from dstack.ai as a CSV file
2. Fetch the dataset from Python or R using the `dstack` package:

{% tabs %}
{% tab title="Python" %}
```python
import pandas as pd
import dstack as ds

df = ds.pull("/<username>/<stackname")
head(df)
```
{% endtab %}

{% tab title="R" %}
```r
library(dstack)    

df <- read.csv(dstack::pull("/<username>/<stackname")
head(df)
```
{% endtab %}
{% endtabs %}

Note, in case you'd like to pull a dataset that is associated with a specific parameter, you have to specify this parameter as an argument of the `pull` function. Here's an example:

{% tabs %}
{% tab title="Python" %}
```python
import pandas as pd
import dstack as ds

df = ds.pull("/cheptsov/player_data", College = "Top 10 colleges")
df.head()
```
{% endtab %}

{% tab title="R" %}
```r
library(dstack)    

df <- read.csv(dstack::pull("/cheptsov/player_data", College = "Top 10 colleges")
head(df)
```
{% endtab %}
{% endtabs %}

## GeoPandas support

You can also push and pull GeoDataFrame from [GeoPandas](https://geopandas.org/):

```python
import geopandas
import pandas as pd
import dstack as ds

df = pd.DataFrame({'City': ['Buenos Aires', 'Brasilia', 'Santiago', 'Bogota', 'Caracas'],
                   'Country': ['Argentina', 'Brazil', 'Chile', 'Colombia', 'Venezuela'],
                   'Latitude': [-34.58, -15.78, -33.45, 4.60, 10.48],
                   'Longitude': [-58.66, -47.91, -70.66, -74.08, -66.86]})

gdf = geopandas.GeoDataFrame(
    df, geometry=geopandas.points_from_xy(df.Longitude, df.Latitude))

ds.push("my_first_geo", gdf)
```

To pull the GeoDataFrame object just call `my_gdf = pull("my_first_geo")`.

