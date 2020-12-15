# Configuration

Uploading datasets and visualization to dstack.ai is done via the `dstack` package available for both Python and R. These packages can be used from Jupyter notebooks, RMarkdown, Python and R scripts and applications.

{% hint style="success" %}
Once you've pushed your data to dstack.ai using the `dstack` packages for Python and R, you can combine your datasets and visualizations into a great-looking interactive dashboards with just a few clicks. [Learn more on how to push your data to dstack.ai](../pushing-visualizations.md)
{% endhint %}

## Installing dstack package

The `dstack` package can be easily installed on of the following ways:

{% tabs %}
{% tab title="pip" %}
```bash
pip install dstack
```
{% endtab %}

{% tab title="conda" %}
```bash
conda install dstack -c dstack.ai
```
{% endtab %}

{% tab title="R" %}
```r
install.packages("dstack")
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Sometimes pip can't resolve `PyYAML` dependency, so it should be installed manually:`pip install pyyaml`
{% endhint %}

## Configuring dstack.ai profile

{% hint style="info" %}
Note, before using the dstack for Python or R, you have to configure it with your dstack.ai profile by specifying your [dstack.ai](https://dstack.ai) username and token:
{% endhint %}

{% tabs %}
{% tab title="Bash" %}
```bash
dstack config add --token <TOKEN> --user <USER>
```
{% endtab %}

{% tab title="R" %}
```r
dstack::configure(user = "<USER>", token = "<TOKEN>", persist = "global")
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Configuring dstack profiles separately from your code, allows you to make the code safe and not include plain secret tokens.
{% endhint %}

Note, by default, the server stores all the data under `.dstack` in the user home directory. In case you'd like to store the `.dstack` folder in a different place, use the following command:

```text
dstack server start --home <other_directory>
```

In this case, the server will store all the data in `<other_directory>/.dstack/`.

