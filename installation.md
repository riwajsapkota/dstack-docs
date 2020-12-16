# Installation

To install `dstack` locally, use `pip`:

```bash
pip install --index-url https://test.pypi.org/simple/ --upgrade --no-cache-dir --extra-index-url=https://pypi.org/simple/ dstack==0.6dev17
```

Once the package is installed, you have to run the `dstack` server locally:

```bash
dstack server start
```

By default, the server uses the `8080` port. Optionally, you can specify a custom port by using the command line option `--port`:

```bash
dstack server start --port 8081
```

It may take a while to start the server for the first time. After you've started the `dstack` server, you'll see the following output:

```bash
$ dstack server start
To access the application, open this URL in the browser: http://localhost:8080/auth/verify?user=dstack&code=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&next=/

The default profile in "~/.dstack/config.yaml" is already configured. You are welcome to push your data using Python or R packages.

What's next?
------------
- Checkout our documentation: https://docs.dstack.ai
- Ask questions and share feedback: https://discord.gg/8xfhEYa
- Star us on GitHub: https://github.com/dstackai/dstack
```

{% hint style="info" %}
To access the `dstack` server, make sure yo click the URL provided in the output: `http://localhost:8080/auth/verify?user=dstack&code=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&next=/`

Otherwise, if you just open http://localhost:8080/, the application will require you to use your username and password which you haven't yet defined.
{% endhint %}

