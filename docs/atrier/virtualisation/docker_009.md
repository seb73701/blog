---
title: Run Multiple Containers With Docker Compose
description: 
published: true
date: 2023-06-12T19:30:14.503Z
tags: 
editor: markdown
dateCreated: 2023-06-12T19:30:14.503Z
---

# Run Multiple Containers With Docker Compose

Execute multiple containers side by side without interfering with each other

<img src="https://miro.medium.com/max/720/1*mAg1MJxj9iFd84e1RdcR3g.webp">

_docker compose_

## What is Docker?

Docker has gained popularity for multiple reasons. One of them is creating portable containers which are fast and easy to deploy.

As mentioned on their [website](https://www.docker.com/resources/what-container/): “a container is something that packages your code along with any other dependencies so that it can be deployed across multiple platforms reliably”.

These containers can be run locally on your Windows, Mac, and Linux. Secondly, major cloud systems like AWS or Azure do support them out of the box. Lastly, it can be used on any hosting space where it can be installed and run. If you want to learn more Docker basics and need a cheat sheet for docker CLI, I wrote an introductory article about it [here](https://www.daveops.co.in/post/docker-a-beginner-s-cheat-sheet-2022).

Here we will go deeper to understand some of the advanced features like running multiple containers.

## Docker Compose

With docker-compose one can configure and start multiple containers with a single YAML file. This is really helpful if you are working on a technology stack with multiple technologies. As an example, if you are working on a project that requires MySQL database, python for AI/ML, Node.js for real-time processing & .NET for serving API. It would be cumbersome to set up such an environment for each team member. Docker alleviates this issue with the help of compose.

## Understanding Docker Compose

`docker compose` is a YAML file in which we can configure different types of services. Then with a single command, all containers were built and fired up. There are mainly 3 steps involved

* Generate a Dockerfile for each project.
* Setup services in `docker-compose.yml` file.
* Fire up the containers.

We are now going to implement a solution for the above problem statement.

## Prerequisites

One might expect to have all the technologies installed to run the above-discussed tech stack of MySQL, Python, NodeJS, .NET and PHP. Rather all you need is a docker engine running. The latest versions of docker come with docker-compose installed. For now, no other installation is required.

## Roadmap
Before we start here is just a brief of what we are going to do. We will be tackling each technology one by one. For each technology, we will create a sample application (except MySQL) and create a Dockerfile for each. Then we will point this Dockerfile in our docker-compose YAML file. Lastly, we will configure docker-compose so that each application does what it’s supposed to do.

Before we get started to create a folder name super-app. Secondly, create a `docker-compose.yml` file. In this file, we will configure all our applications. So let’s get started!

For those of you interested in code you can visit the [repository here](https://github.com/shenanigan/super-app-docker).

## How to Configure MySQL with Docker

Set the below content in your `docker-compose.yml` file.

```yaml
version: '3.4'
services:
  super-app-db:
    image: mysql:8.0.28
    environment:
      MYSQL_DATABASE: 'super-app'
      MYSQL_ROOT_PASSWORD: '$SuperApp1'
    ports:
      - '3306:3306'
    expose:
      - '3306'
```

Under the `services` section we will list down all the types of applications to be configured. To start with we configure a `super-app-db` service that pulls a docker image of MySQL with version 8.0.28. Next, we instruct the container to create a database name `super-app` with `root` as default user and it’s password set to `$SuperApp1`.

Lastly, since the default port for MySQL is 3306 we are mapping it to the containers’ port 3306 and exposing that port for access.

Once the above file is created run the below command to get your docker image created and run it as a container.

```
docker compose up
```

MySQL image will be pulled and then the docker will spin up a container to run this image. MySQL server can be verified by connecting it via a MySQL client. If not, worry not, we will see below how to connect to it via one of our applications. As long as the container is not deleted the tables will be persisted.

Let’s configure our next application NodeJS.

## How to Configure NodeJS with Docker

We will create a very simple express node application. In order to do so create a folder named `node` inside our `super-app` folder. Add the below 3 files namely `server.js`, `package.json` and `Dockerfile` in the node folder.

`server.js`:

```javascript
const server = require("express")();
server.listen(3000, async () => { });
server.get("/super-app", async (_, response) => {
    response.json({ "super": "app" });
});
```

`package.json`:

```json
{
    "name": "super-app-node",
    "dependencies": {
        "express": "^4.17.1"
    }
}
```

`Dockerfile`:

```docker
# Download the slim version of node
FROM node:17-slim
# Set the work directory to app folder. 
# We will be copying our code here
WORKDIR /node
#Copy package.json file in the node folder inside container
COPY package.json .
# Install the dependencies in the container
RUN npm install
# Copy the rest of the code in the container
COPY . .
# Run the node server with server.js file
CMD ["node", "server.js"]
# Expose the service over PORT 3000
EXPOSE 3000
```

Here we are creating a node application that returns JSON when we hit `localhost:3000/super-app` in the browser. Now, we are not directly going to run the project from this folder.

Instead, go back to your `super-app` folder and append the below lines to your `docker-compose.yml` file.

```docker
super-app-node:
    build: ./node
    ports:
      - "3000:3000"
```

We are simply mentioning creating a service named `super-app-node`. We are also mapping the container port to the host port 3000. Finally, run the below command to run your two containers i.e. MySQL & NodeJS.

```
docker compose up
```

Now if you hit `localhost:3000/super-app` you will see a response `{“super”:”app”}`. Simultaneously your MySQL service is also. Yay! We have successfully created two containers using a docker-compose file.

On to the next application. Let’s create a .NET application that interacts with the database and returns a list of strings.

## How to Configure .NET 6.0 with Docker

We want the .NET application to connect with the database. Secondly, it will fetch data from the database via a GET API and display it in the browser. To do so create a .NET project named `dotnet` inside our `super-app` project.

Since writing down all the steps for creating and setting up the project will be quite lengthy for this blog and also keep this article focused on docker, a separate [article](https://arjavdave.com/2022/04/17/code-first-entity-framework-core-mysql/) is written on how to achieve the above goal.

The [article](https://arjavdave.com/2022/04/17/code-first-entity-framework-core-mysql/) provides step-by-step instructions on creating a .NET 6 application that connects to MySQL database using EF Core. EF Core is an Object-Relation Mapper to communicate between database and code. **Note:** _Ignore the prerequisites section since we already have a MySQL server running in docker._

## .NET App Changes

Since the projects will be running in their own containers they can’t communicate via `localhost` url. Hence in order for the .NET app to communicate with MySQL, the connection string needs to be updated. To do so open the `appsettings.json` and `appsettings.Development.json` files and replace `localhost` with `super-app-db` for _server_ in `DefaultConnection`.

Lastly, apps within containers don’t need HTTPS redirection. Since the HTTPS should be handled by the server remove the below line from `Program.cs`.

```csharp
app.UseHttpsRedirection();
```

## Docker

Once the project is set up and running it is time to configure it to run inside a docker using Dockerfile and docker-compose. In `dotnet` folder create `Dockerfile` with the below contents.


```docker
#Get the SDK image to build and publish the project
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
WORKDIR /app
# Copy everything
COPY . ./
# Restore as distinct layers
RUN dotnet restore
# Build and publish a release
RUN dotnet publish -c Release -o out
# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
#Copy the build file to the app directory
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "dotnet.dll"]
#Expose the port for communication
EXPOSE 80
```

Now go back to the docker-compose.yml file and append the below content.

```docker
super-app-dotnet:
    build: ./dotnet
    ports:
    - "8080:80"
```

Here we are binding port 8080 of the host machine to the container’s port 80. That’s all for now. Execute the below command to fire up all the containers.

```
docker compose up
```

Finally, hit [localhost:8080/api/job](http://127.0.0.1:8080/api/job) in a browser. The GET API will fetch the list of jobs from the database.

## How to Configure Python with Docker

By now, you might have guessed to create a python folder in our `super-app` folder.

Secondly, create the three files required for our project namely `ai-ml.py`, `requirements.txt` and `Dockerfile` with the below contents.

`ai-ml.py`:

```python
import matplotlib.pyplot as plt
import pandas as pd
from scipy import signal
if __name__ == "__main__":
    print("All working good")
```

`requirements.txt`:

```
pandas
scipy
matplotlib
```

`Dockerfile`:

```docker
# Get the python image
FROM python:3.7.13-slim
# Switch to app directory
WORKDIR /app
# Copy the requirements in to the app
COPY requirements.txt ./
# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt
# Copy everything else
COPY . .
#Run the python script
CMD [ "python", "./ai-ml.py" ]
```

Finally, go back to the `docker-compose.yml` file and add the below content.

```docker
super-app-python:
    build: ./python
```

It’s as simple as that. Since it’s just a simple script it will run once and then the container will exit. But the logs for the container will show All working well printed. That’s all for Python.

## How to Configure PHP with Docker

Setting up PHP with docker is one of the easiest of all. Create two files `index.php` and `Dockerfile` as below.

`index.php`:

```php
<?php echo "I am running in a container."; ?>
```

`Dockerfile`:

```docker
# Get the php apache image
FROM php:8.0-apache
# Switch to app directory
WORKDIR /var/www/html
# Copy everything
COPY . .
EXPOSE 80
```

Lastly, add the below content to `docker-compose.yml`.

```docker
super-app-php:
    build: ./php
    ports:
    - "8000:80"
```

Finally, fire up all the containers again with `docker compose up`. When you hit http://localhost:8000 a nice message saying “I am running in a container.” will appear.

Here is the final [repository](https://github.com/shenanigan/super-app-docker) with all the Dockerfiles and other setups.

## Conclusion

Docker is a wonderful containerization tool and it’s been made more powerful with docker-compose. It allows running multiple containers side by side without interfering with each other. It should be in the arsenal of tools in the knowledge base.

Source : https://betterprogramming.pub/run-multiple-containers-with-docker-compose-9297957f7a3c