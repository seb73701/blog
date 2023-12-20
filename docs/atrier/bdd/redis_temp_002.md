---
title: Installing Redis Server Using Docker Container
description: 
published: true
date: 2023-07-16T07:40:50.887Z
tags: 
editor: markdown
dateCreated: 2023-07-16T07:40:50.887Z
---

# Installing Redis Server Using Docker Container

Installing a Redis server is not too hard following [this guide](https://redis.io/topics/quickstart). But it is much easier simply running it from a [Docker](https://en.wikipedia.org/wiki/Docker_(software)) container.

## Prerequisites
To use Docker on your laptop, you need to install [Docker Desktop](https://www.docker.com/products/docker-desktop). There is a version for Mac, Windows, and Linux.

## Step 1 — Install and Run the container
Simply execute the following command:

```shell
docker run --name my-redis -p 6379:6379 -d redis
```

This command will do the following:

1. Pull the latest Redis image from the [Docker hub](https://hub.docker.com/_/redis) (the place where all the public 3rd party images are stored)
1. Create and run the container and name it: **my-redis**
1. Route port **6379** on my laptop to port **6379** inside the container. **6379** is Redis default port and can be changed

That’s it!

> Please note: if the Redis image already exists on your local registry, it won’t be pulled again. So the next time you run this command, it will be much fatser

```shell
➜ docker run --name my-redis -p 6379:6379 -d redis
Unable to find image 'redis:latest' locally
latest: Pulling from library/redis
33847f680f63: Pull complete
26a746039521: Pull complete
18d87da94363: Pull complete
5e118a708802: Pull complete
ecf0dbe7c357: Pull complete
46f280ba52da: Pull complete
Digest: sha256:cd0c68c5479f2db4b9e2c5fbfdb7a8acb77625322dd5b474578515422d3ddb59
Status: Downloaded newer image for redis:latest
eff7dedc53ec235068d543574111055a08d84b518f149ba6aac4adb6e63181d4
```

We can confirm Redis Docker is running by using [docker ps](https://docs.docker.com/engine/reference/commandline/ps/):

```shell
➜ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
eff7dedc53ec   redis     "docker-entrypoint.s…"   34 minutes ago   Up 28 minutes   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   my-redis
```

We can see a container by the name of **my-redis** which is up snd running for **28 minutes**.

## Step 2 — Connect to Redis from inside the container
First, let’s try to connect from inside the container. Simply run the following command:

```shell
docker exec -it my-redis sh
```

This command will do the following:

1. Run the shell inside the Redis container by the name of my-redis
1. Display the command prompt

```shell
➜ docker exec -it my-redis sh
#
```

We can execute [redis-cli](https://redis.io/topics/rediscli) (Redis command-line interface) and do some tests:

```shell
# redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set name Monica
OK
127.0.0.1:6379> get name
"Monica"
127.0.0.1:6379>
```

## Step 3 — Connect to Redis from your laptop
Now, let’s see if we can also connect to Redis from my laptop. First, we need [redis-cli](https://redis.io/topics/rediscli) (Redis command-line interface) installed locally.

Then, simply do the same tests again:

```shell
➜ redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> get name
"Monica"
127.0.0.1:6379>
```

That’s it! we now have Redis installed locally via Docker. 🙌

## What about persistency?
As long as we don’t remove the container, the data will remain intact. Let’s test this by:

1. Stopping the Container
1. Starting the Container
1. Checking key value

```shell
➜ docker stop my-redis
my-redis
➜ redis-cli
Could not connect to Redis at 127.0.0.1:6379: Connection refused
not connected> exit
➜ docker start my-redis
my-redis
➜ redis-cli
127.0.0.1:6379> get name
"Monica"
127.0.0.1:6379>
```

source : https://medium.com/idomongodb/installing-redis-server-using-docker-container-453c3cfffbdf