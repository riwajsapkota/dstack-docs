# Sharing

`dstack` supports two ways to share models and applications with others: inviting users to your `dstack` server or pushing your applications to [dstack.cloud](https://dstack.cloud).

#### Own Server

Inviting users to your `dstack` server can be done from the `Settings`: 

![](../.gitbook/assets/screenshot-2021-01-15-at-09.54.19.png)

You can create a user, copy its invite link, and send it to the user over email or some other way.

Pushing applications to [dstack.cloud](https://dstack.cloud) is also easy and is don the following way:

1. Sign up for [dstack.cloud](https://dstack.cloud)
2. Open Settings, click the `Information` icon next to your `Token`, and copy the shell command that configures your `dstack` client. 
3. Run the copied shell command snippet on the machine where you push applications or models. This command will change your `~/.dstack/config.`yaml to configure it to use dstack.cloud instead of your local server.
4. Push models and applications as you normally do. They'll be pushed to [dstack.cloud](https://dstack.cloud).
5. If you'd like to change the visibility settings of your applications or models, you can do it by clicking the Share button on the page of the corresponding application or model.



