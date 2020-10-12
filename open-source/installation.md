# Installation

The main components of dstack include:

* Client packages for Python \([dstack-py](https://github.com/dstackai/dstack-py)\) an R \([dstack-r](https://github.com/dstackai/dstack-r)\). These packages can be used from either notebooks or scripts to push data to dstack.
* A server application \([dstack](https://github.com/dstackai/dstack-server)\). It handles the requests from the Client packages, and serve data applications. The application can run locally or in Docker. 

The easiest way to install `dstack` is by using `pip` or `conda`:

{% tabs %}
{% tab title="pip" %}
```bash
pip install dstack
```
{% endtab %}

{% tab title="conda" %}
```text
conda install dstack -c dstack.ai
```
{% endtab %}
{% endtabs %}

The package comes with a command-line tool called `dstack`. This command-line tool can be used to configure local profiles, credentials, and to run a local server.

If you're using R and don't need the command-line tool, you can install the client package for R via the following command:

```r
install.packages("dstack")
```

## Run a server

In order to run a server locally, one must run this command line:

```bash
dstack server start
```

You'll see the following output:

```text
To access the dstack server, open one of these URLs in the browser:
        http://localhost:8080/auth/verify?user=dstack&code=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&next=/
    or    http://127.0.0.1:8080/auth/verify?user=dstack&code=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&next=/

If you're using Python, use the following command line command to configure your dstack profile:
    pip install dstack
    dstack config add --token xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --user dstack --server http://localhost:8080/api

If you're using R, use the following R command to configure your dstack profile:
    install.packages("dstack")
    dstack::configure(user = "dstack", token = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", persist = "global", server = "http://localhost:8080/api")
```

Note, in your case instead of `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` you'll see your personal code.

The server by default uses the `8080` port. Optionally, you can specify a custom port by using the command line option `--port`:

```bash
dstack server start --port 8081
```

Note, by default, the server stores all the data under `.dstack` in the user home directory. In case you'd like to store the `.dstack` folder in a different place, use the following command:

```bash
dstack server start --home <other_directory>
```

In this case, the server will store all the data in `<other_directory>/.dstack/`.

## Configure a user profile

In order to send requests to the locally running server, one must run the command suggested in the output:

```bash
dstack config add --token xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --user dstack --server http://localhost:8080/api
```

{% hint style="info" %}
Note, if you don't want to run a server yourself, you can use the in-cloud version running on dstack.ai. To configure a profile that uses dstack.ai, you have to sign up for a [dstack.ai](https://dstack.ai) account, go to settings, and copy the username and the client token. The server in that case must not be specified.
{% endhint %}

{% hint style="danger" %}
Note, the R CRAN package is still under review. In order to install it, please use the following commands:

```r
install.packages(c('uuid', 'bit64', 'rjson', 'rlist'), repos = 'http://cran.us.r-project.org')
install.packages('https://drive.google.com/uc?export=download&id=1RREfEk_rZFvZN-vS-7H0oPIIWJtWBMar', repos = NULL, type = 'source')
```
{% endhint %}

