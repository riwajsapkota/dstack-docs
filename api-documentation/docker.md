# Docker

{% hint style="info" %}
You can find dstack for Docker on DockerHub here - [https://hub.docker.com/r/dstackai/dstack](https://hub.docker.com/r/dstackai/dstack)
{% endhint %}

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

By default, it uses the built-in SQLite database to store data. If you want, you can connect the Docker image to your existing Cassandra cluster. To do that, use the following environment variables:

`-e dstack_cassandra_hosts=<comma-separated list of hosts> -e dstack_cassandra_port=<port> -e dstack_cassandra_user=<user> -e dstack_cassandra_password=<password>`

In this case the Docker image will use the `dstack` keyspace. If it doesn't exist, it will create it. The Docker image handles schema evolution automatically.

{% hint style="info" %}
If you'd like to test how the Docker image works without configuring an SMTP server, you can try to run it via the [docker-compose.yml](https://github.com/dstackai/dstack-docker/blob/master/docker-compose.yaml).
{% endhint %}

In case you don't want to use Docker or the multi-user version of dstack, you also can use [github.com/dstackai/dstack](https://github.com/dstackai/dstack) to start a dstack server locally via command line.

