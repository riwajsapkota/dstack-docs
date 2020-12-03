# Applications \(Beta Version\)

{% hint style="info" %}
Applications is a new upcoming feature of [github.com/dstackai/dstack](https://github.com/dstackai/dstack) that allows Python developers to quickly build, run, and share data applications. A limited preview of this feature is already available. Please read on this page on how it works and how to try it.
{% endhint %}

Often, the result of data scientists' activities needs to be repeatedly shared with other colleagues. There are a number of ways to do that:

* Jupyter notebooks \(sending sources and outputs within a notebook\)
* Static images \(sending an image over email or Slack\)
* Self-service applications \(building web applications others can use\)

To build such self-service applications, data scientists often have to use Flask, Django, or Plotly Dash that require a lot of effort to develop these applications and then deploy them.

The new feature of `dstack` helps data scientists build such applications and share them with the team in minutes.

### Installation

{% hint style="warning" %}
In order to try the new feature, you have to install the **`0.6dev9`** version of the `dstack` Python package from [test.pypi.org](https://test.pypi.org/):
{% endhint %}

```bash
pip install --index-url https://test.pypi.org/simple/ --upgrade --no-cache-dir --extra-index-url=https://pypi.org/simple/ dstack==0.6dev11
```

Once the package is installed, you have to run the `dstack` server locally:

```bash
dstack server start
```

By default, the server uses the `8080` port. Optionally, you can specify a custom port by using the command line option `--port`:

```bash
dstack server start --port 8081
```

After you've started the `dstack` server, you can use the `dstack`'s API from a Python script or a Jupyter notebook.

### A Minimal Application

Here is an example of a very simple application. The application takes real-time stock exchange data from Yahoo Finance for the FAANG companies and renders it. The user is prompted to choose one of the companies to view its latest market data in form of a candlestick chart.

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

If we click the URL, we'll see the following application:

![](https://i.ibb.co/QCvkLBX/Screenshot-2020-11-27-at-17-00-57.png)

With this application, the user can select different symbols and see the latest market data on a chart.

### A More Complex Application

Here's a more complex application example. In this application, the user will see two tabs. In the first tab, the user will be prompted to choose a region and a country, then to see a list of companies and their data. The list of countries will depend on the selected region. In order to see the data, the user will have to confirm the selection by clicking the `Apply` button. In the second tab, the user will be able to see a chart with numbers of purchased licenses per year for the selected country.

![](https://i.ibb.co/ctB9kGY/Screenshot-2020-11-27-at-17-13-35.png)

Here's the full code of the application:

```python
import dstack.controls as ctrl
import dstack as ds
import pandas as pd
import plotly.express as px


@ds.cache()
def get_data():
    return pd.read_csv("https://www.dropbox.com/s/cat8vm6lchlu5tp/data.csv?dl=1", index_col=0)


def get_regions():
    df = get_data()
    return df["Region"].unique().tolist()


def get_countries_by_region(self: ctrl.ComboBox, regions: ctrl.ComboBox):
    df = get_data()
    self.data = df[df["Region"] == regions.value()]["Country"].unique().tolist()


regions = ctrl.ComboBox(data=get_regions, label="Region")
countries = ctrl.ComboBox(data=get_countries_by_region, label="Country", depends=[regions])


def get_data_by_country(regions: ctrl.ComboBox, countries: ctrl.ComboBox):
    df = get_data()
    return df[df["Country"] == countries.value()]


data_by_country_app = ds.app(get_data_by_country, regions=regions, countries=countries)


def get_companies():
    df = get_data()
    return df["Company"].unique().tolist()


companies = ctrl.ComboBox(data=get_companies, label="Company", require_apply=False)


@ds.cache()
def get_data_by_company(companies: ctrl.ComboBox):
    df = get_data()
    row = df[df["Company"] == companies.value()].filter(["y2015", "y2016", "y2017", "y2018", "y2019"], axis=1)
    row.rename(columns={"y2015": "2015", "y2016": "2016", "y2017": "2017", "y2018": "2018", "y2019": "2019"},
               inplace=True)
    col = row.transpose()
    col.rename(columns={col.columns[0]: "Licenses"}, inplace=True)
    fig = px.bar(col, labels={"value": "Licenses", "index": "Year"})
    fig.update(layout_showlegend=False)
    return fig


data_by_company_app = ds.app(get_data_by_company, companies=companies)

frame = ds.frame("comp_data")
frame.add(data_by_country_app, params={"Companies": ds.tab()})
frame.add(data_by_company_app, params={"Licenses": ds.tab()})
result = frame.push()
print(result.url)
```

Let's take a closer look at every step.

In this example, for every tab, we create a separate application and push it along with the name of the tab within a single frame to the `dstack` server.

Let's start with the first tab.

**Data**

First, we define a function `get_data` which fetches data from an external source and returns a pandas dataframe with this data.

```python
@ds.cache()
def get_data():
    return pd.read_csv("https://www.dropbox.com/s/cat8vm6lchlu5tp/data.csv?dl=1", index_col=0)
```

{% hint style="info" %}
Note, in order to improve the performance of our application, we use the annotation `@dstack.cache`. This annotation wraps the function to make sure that the result of the function is cached, so when it's called the next time, the function is not called again. 
{% endhint %}

**User controls**

Second, we define `get_regions` that returns regions that appear in the data. This function is going to be used to populate the values of the combo box `"Region"`.

```python
def get_regions():
    df = get_data()
    return df["Region"].unique().tolist()
```

Then, we define a function `get_countries_by_region` that updates the combo box `Country` based on the selection of the combo box `Region`.

```python
def get_countries_by_region(self: ctrl.ComboBox, regions: ctrl.ComboBox):
    df = get_data()
    self.data = df[df["Region"] == regions.value()]["Country"].unique().tolist()
```

After that, we define our combo boxes `Region` and `Country`. Notice, that the second combo box depends on the first combo box. This is specified by the attribute `depends_on` provided by `dstack.controls.Control` \(and its subclasses such as `dstack.controls.ComboBox`\).

```python
regions = ctrl.ComboBox(data=get_regions, label="Region")
countries = ctrl.ComboBox(data=get_countries_by_region, label="Country", depends=[regions])
```

Since we don't use the set attribute `require_apply` to `False` in the controls, the resulting application will prompt the user to click the `Apply` button to see the output of the application.

**Application output**

Now, we can define the function `get_data_by_country` that produces the output of our application based on selected values in our combo boxes.

```python
def get_data_by_country(regions: ctrl.ComboBox, countries: ctrl.ComboBox):
    df = get_data()
    return df[df["Country"] == countries.value()]
```

**Application**

Finally, we create an application by using the `dstack.app` function and passing our function `data_by_country_app` and binding its arguments to our controls `regions` and `countries`.

```python
data_by_country_app = ds.app(get_data_by_country, regions=regions, countries=countries)
```

The application for the first tab is ready, let's move on and create the other one.

**Data**

We start by defining the function `get_companies`. This function is going to be used to populate the combo box `Company`.

```python
def get_companies():
    df = get_data()
    return df["Company"].unique().tolist()
```

**User controls**

Then, we define this combo box and set `require_apply` to `False` so the tab doesn't require the user to click `Apply` to see the result.

```python
companies = ctrl.ComboBox(data=get_companies, label="Company", require_apply=False)
```

**Application output**

Then, we define a function `get_data_by_company` that produces a Plotly chart with numbers of purchased licenses for the selected company grouped by the year.

```python
@ds.cache()
def get_data_by_company(companies: ctrl.ComboBox):
    df = get_data()
    row = df[df["Company"] == companies.value()].filter(["y2015", "y2016", "y2017", "y2018", "y2019"], axis=1)
    row.rename(columns={"y2015": "2015", "y2016": "2016", "y2017": "2017", "y2018": "2018", "y2019": "2019"},
               inplace=True)
    col = row.transpose()
    col.rename(columns={col.columns[0]: "Licenses"}, inplace=True)
    fig = px.bar(col, labels={"value": "Licenses", "index": "Year"})
    fig.update(layout_showlegend=False)
    return fig
```

{% hint style="info" %}
Here, we also use the annotation `@dstack.cache` to prevent the function to be called when it is not necessary.
{% endhint %}

**Application**

Now, we're ready to create the second application by using the `dstack.app` function, where we pass our function `get_data_by_company` and bind the name of its argument to the combo box.

```text
data_by_company_app = ds.app(get_data_by_company, companies=companies)
```

#### Deploy applications

Finally, we are ready to push both applications and tabs to the dstack server. Since we push more than one application, we have to first create a frame \(by using the `dstack.frame` function\), and then adding our applications \(by using the `dstack.stack.StackFrame.add` function\).

```python
frame = ds.frame("comp_data")
frame.add(data_by_country_app, params={"Companies": ds.tab()})
frame.add(data_by_company_app, params={"Licenses": ds.tab()})
```

Notice, to associate an application with a tab, we use the attribute `params` of the `add` function. There, the name of the param is supposed to be the title of the tab. The value of the param must be set to `dstack.tab()`.

The last thing is pushing the frame with `dstack.stack.StackFrame.push`.

```python
result = frame.push()
print(result.url)
```

Just like in the previous example, once the application is pushed, we can click the URL and see the application:

With this application, the user may switch between tabs, change the controls, and see the updating outputs.

### FAQ

**1. How does dstack compliment Jupyter notebook and other tools?**

The purpose of `dstack`'s applications is not to replace Jupyter notebooks \(`dstack` can be used from `Jupyter` notebooks\) but rather to build in-house data applications for non-technical colleagues within the company.

In other words, dstack is a platform for building internal in-house data applications, e.g. for advanced analysis or prediction in business units as an addition to normal business intelligence tools \(Tableau, `Power BI`, `QuilkSense`, `Metabase`, `Superset`, etc\).

**2. Which application outputs are supported?**

It supports `pandas`, `matplotlib`, `Plotly`, `Bokeh`, and `Seaborn`.

**3. What type of controls are supported?**

* [x] `dstack.controls.TextField`
* [x] `dstack.controls.ComboBox`
* [x] `dstack.controls.Slider`

The following controls are going to be available soon:

* [ ] `dstack.controls.CheckBox`
* [ ] `dstack.controls.MultiSelect`
* [ ] `dstack.controls.TextArea`
* [ ] `dstack.controls.FileUploader`

**4. How does dstack help with building ML applications?**

The dstack framework offers an [ML Registry](../tutorials/machine-learning-models/) where you can push your models and later pull them to be used within applications. It supports `Scikit-learn`, `Tensorflow`, and `PyTorch` models.

**5. How does this new feature compare to other existing features of dstack?**

It's very likely that the other existing features of dstack such as Stacks, Reports, Jobs, etc, are going to be discontinued after the public release of interactive applications.

 **6. What are the other limitations of current implementation?**

* [ ] Application outputs do not support complex layouts yet.
* [ ] Applications work only with the open-source version. The cloud version of dstack is going to be [discontinued](https://blog.dstack.ai/discontinuing-cloud-and-r-to-fully-focus-on-open-source-data-applications-using-python).
* [ ] Reports are not compatible with Applications. It's likely that Reports might be discontinued after the release of Applications.

### Need Help?

Do you have any feedback either minor or critical? Something doesn't work? Have a question? Please, write to us on our [Discord Channel](https://discord.gg/8xfhEYa).

