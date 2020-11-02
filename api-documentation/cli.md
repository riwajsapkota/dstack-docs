---
description: This is the API Reference for using the dstack Command Line Tool.
---

# CLI

Please make sure you have followed the [Installation procedure](../open-source/installation.md) before referring to the API.

{% hint style="success" %}
You can find the complete open source implementation of the dstack CLI here -[https://github.com/dstackai/dstack-py/tree/master/dstack/cli](https://github.com/dstackai/dstack-py/tree/master/dstack/cli)
{% endhint %}

The dstack CLI has **2 optional arguments** - 

`dstack --help`- Show help message and exit

`dstack --version`- Show program's version number and exit

and it has **2 positional arguments -** 

`dstack config`- To manage the user profile configuration.

`dstack server` - To manage your local dstack server.

These positional arguments **\(Config** and **Server\)** are elaborated below.

## Config

To manage your user profile configuration.

{% hint style="info" %}
You can always use `-h or --help` to show the help message for all parameters.
{% endhint %}

### 1. Add

To add a user profile

```bash
dstack config add [-h] [--token [TOKEN]] [--server [SERVER]]
                         [--user [USER]] [--no-verify] [--force] [--file FILE]
                         [PROFILE]
```

{% tabs %}
{% tab title="Optional Parameters" %}
`-h, --help`  - show this help message and exit

`--token [TOKEN]`- set token for selected profile

`--server [SERVER]` - set server to handle api requests

`--user [USER]`- set user name

`--no-verify`- do not verify SSL certificates

`--force` - don't ask for confirmation

`--file FILE` - use specific config file
{% endtab %}

{% tab title="Positional Parameters" %}
`[PROFILE]`- Profile name, `'default'` if missing
{% endtab %}
{% endtabs %}

### 2. Modify

```bash
dstack config modify [-h] [--token [TOKEN]] [--server [SERVER]]
                         [--user [USER]] [--no-verify] [--force] [--file FILE]
                         [PROFILE]
```

{% tabs %}
{% tab title="Optional Parameters" %}
`-h, --help`  - show this help message and exit

`--token [TOKEN]`- set token for selected profile

`--server [SERVER]` - set server to handle api requests

`--user [USER]`- set user name

`--no-verify`- do not verify SSL certificates

`--force` - don't ask for confirmation

`--file FILE` - use specific config file
{% endtab %}

{% tab title="Positional Parameters" %}
`[PROFILE]`- Profile name, `'default'` if missing
{% endtab %}
{% endtabs %}

### 3. Remove

Remove existing user profile

```bash
dstack config remove [-h] [--force] [--file FILE] [PROFILE]
```

{% tabs %}
{% tab title="Optional Parameters" %}
`-h, --help`  - show this help message and exit

`--force` - don't ask for confirmation

`--file FILE` - use specific config file
{% endtab %}
{% endtabs %}

### 4. List

List all configured user profiles

```bash
dstack config list [-h] [--file FILE] 
```

{% tabs %}
{% tab title="Optional Parameters" %}
`-h, --help`  - show this help message and exit

`--file FILE` - use specific config file
{% endtab %}
{% endtabs %}

## Server

To manage your dstack server.

{% hint style="info" %}
You can use `-h, --help` to show the help message for all pameters.
{% endhint %}

### 1. start

Start a server

```bash
dstack server start [-h] [--port [PORT]] [--home [HOME]] [--skip]
                           [--no-verify]
```

{% tabs %}
{% tab title="Optional Parameters" %}
`--port [PORT]` -  use specific port

`--home [HOME]` - store server data in the specified directory

`--skip` - skip checking for updates

`--no-verify`  - do not verify SSL certificates
{% endtab %}
{% endtabs %}

### 2. version

Prints server version

```bash
dstack server version 
```

### 3.update

Update server version

```bash
dstack server update 
```

