# FAQ

### How does dstack compliment Jupyter notebook and other tools?

The purpose of `dstack`'s applications is not to replace Jupyter notebooks \(`dstack` can be used from `Jupyter` notebooks\) but rather to build in-house data applications for non-technical colleagues within the company.

In other words, dstack is a platform for building internal in-house data applications, e.g. for advanced analysis or prediction in business units as an addition to normal business intelligence tools \(Tableau, `Power BI`, `QuilkSense`, `Metabase`, `Superset`, etc\).

### Which application outputs are supported?

It supports `pandas`, `matplotlib`, `Plotly`, `Bokeh`, and `Seaborn`.

### What type of controls are supported?

* [x] `dstack.controls.TextField`
* [x] `dstack.controls.ComboBox`
* [x] `dstack.controls.Slider`
* [x] `dstack.controls.CheckBox`

The following controls are going to be available soon:

* [ ] `dstack.controls.FileUploader`

### How does dstack help with building ML applications?

The dstack framework offers an ML Registry where you can push your models and later pull them to be used within applications. It supports `Scikit-learn`, `Tensorflow`, and `PyTorch` models.

### What are the other limitations of current implementation?

* [ ] Application outputs do not support complex layouts yet.
* [ ] Applications work only with the open-source version. The cloud version of dstack is going to be [discontinued](https://blog.dstack.ai/discontinuing-cloud-and-r-to-fully-focus-on-open-source-data-applications-using-python).

