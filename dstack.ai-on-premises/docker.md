# Running in Docker

The [Docker image](https://hub.docker.com/repository/docker/dstackai/dstack) can be used to host the enterprise version of dstack.ai on own servers or in own AWS/Azure/Google Cloud account. 

You can run dstack.ai in Docker using the following command:

```bash
docker run -d --name <dstack container name> \
-v <path to data directory>:/data \
-p <port on host>:8080 -e dstackai_port=<public port> \
-e dstack_smtp_host=<SMTP server host> \
-e dstack_smtp_port=<SMTP server port> \
-e dstack_smtp_user=<SMTP server user> \
-e dstack_smtp_password=<SMTP server password> \
-e dstack_smtp_starttls=true \
-e dstack_smtp_from=<admin email address> \
dstackai/dstack:latest
```

To stop, the server, make sure to use this code:

```bash
docker stop <dstack container name>
```

If you'd like to re-run the server after it was stopped, make sure to delete it before running again using this command:

```bash
docker rm <dstack container name>
```

{% hint style="info" %}
If you'd like to test how the Docker image works without configuring an SMTP server, you can try to run it via the [docker-compose.yml](https://github.com/dstackai/dstack-docker/blob/master/docker-compose.yaml).
{% endhint %}

