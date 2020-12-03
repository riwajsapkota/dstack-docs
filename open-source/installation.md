---
description: 'Before you can use dstack, you’ll need to get it installed.'
---

# Installation

There are 2 main components of dstack:

* **Client packages** for Python \([dstack-py](https://github.com/dstackai/dstack-py)\) an R \([dstack-r](https://github.com/dstackai/dstack-r)\). These packages can be used from either notebooks or scripts to push data to dstack.
* **A server application** \([dstack](https://github.com/dstackai/dstack-server)\). It handles the requests from the Client packages, and serves data applications. The application can run locally or in Docker, or you can use the in-cloud version running on [dstack.ai](https://dstack.ai) if you don't want to run the server yourself.

## Installing Client Packages

### Python

The easiest way to install `dstack` in Python is by using `pip`:

{% tabs %}
{% tab title="pip" %}
```bash
pip install dstack
```
{% endtab %}
{% endtabs %}

The python package **comes** **with** **a command-line tool** called `dstack`. This command-line tool can be used to configure local profiles, credentials, and to run a local server.

### R

If you're using R and don't need the command-line tool, you can install the client package for R via the following command:

```r
install.packages("dstack")
```

### Quick Start

Once you are done installing the Client Packages, the easiest way to start using dstack would be to Register for [dstack.ai cloud](https://dstack.ai/auth/signup) and [follow the configuration instructions here](../in-cloud/installation.md#configuring-dstack-ai-profile) -

```text
dstack config add --token [YOUR TOKEN] --user [YOUR USERNAME]
```

{% hint style="info" %}
You can find your token number and username information inside settings - [https://dstack.ai/settings](https://dstack.ai/settings)
{% endhint %}

You can read more details about Configuration and dstack.ai cloud here-

{% page-ref page="../in-cloud/installation.md" %}

Otherwise you can also run a server locally as specified below.

## Run a server

{% hint style="info" %}
Note, if you don't want to run a server yourself, you can use the in-cloud version running on dstack.ai. To configure a profile that uses dstack.ai, you have to sign up for a [dstack.ai](https://dstack.ai) account, go to settings, and copy the username and the client token. The server in that case must not be specified.
{% endhint %}

After installing the `dstack` Python Client package, you can access the `dstack` command line tool, which you can use to run a local server.

In order to run a server locally, run this command line:

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

