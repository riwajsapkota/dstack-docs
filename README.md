# What is dstack?

[dstack.ai](https://dstack.ai) is an open-source tool and an enterprise platform for building data and ML applications using Python and R.

How is dstack different from other frameworks \(Shiny, Dash, Streamlit, and others\):

* It is designed for data scientists and doesn't require development skills to build applications.
* It simplifies the process of creating applications by leveraging 
  * a\) a declarative approach to defining application components; 
  * b\) agnostic to data science frameworks and tools.

**How dstack works**

The framework consists of the following parts:

* Client packages for Python \([dstack-py](https://github.com/dstackai/dstack-py)\) an R \([dstack-r](https://github.com/dstackai/dstack-r)\). These packages can be used from either notebooks or scripts to push data to dstack.
* A server application \([dstack-server](https://github.com/dstackai/dstack-server)\). It handles the requests from the Client packages, and serve data applications. The application can run locally or in Docker.

A data science application is a specific kind of application that solves domain-specific problems using data and data science methods. These data science methods may include data-wrangling, data visualizations, statistical modeling, machine learning, etc.

**Building data science applications**

There are several general use-cases for such data science applications:

1. _Interactive reports_ – a set data visualizations and interactive widgets, combined using a certain layout
2. _Live dashboards_ – applications that fetch data from various data sources, turn it into visualizations and combine using a certain layout \(not supported yet\)
3. _Machine learning applications_ – applications that let users interact with ML models \(not supported yet\)

{% page-ref page="open-source/dashboards.md" %}

Currently, dstack supports only _Interactive reports_. The support for _Live dashboards_ and _Machine learning applications_ is coming soon.

{% hint style="info" %}
Check out this [quick tutorial](tutorials/dashboards-tutorial.md) on how to build interactive reports with dstack in minutes.
{% endhint %}

**Automating scientific routines**

The dstack in-cloud and on-premises versions allow you to execute Python and R jobs right – either on demand or automatically at a regular schedule.

{% page-ref page="open-source/jobs.md" %}

The jobs are ideal for processing data regularly and publishing the results, e.g. in the form of data visualizations, datasets, or other artifacts.

