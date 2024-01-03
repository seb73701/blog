---
title: Introduction to Docker - beginners
description: 
published: true
date: 2023-06-17T07:48:09.220Z
tags: 
editor: markdown
dateCreated: 2023-06-17T07:48:09.220Z
---

# Introduction to Docker - beginners

![1_grdippc71coyaxs0-y4hnq.webp](/assets/img/virtualisation/tempo/1_grdippc71coyaxs0-y4hnq.webp)

I remember my third project demo. After building and testing the project, everything worked. So I sent it over to the person that would do the demo. But during the demo, the project was able to run. In my defense, I was like

> “But it works on my computer”

![1_h-weoxv4kwxithcfbhmgia.webp](/assets/img/virtualisation/tempo/1_h-weoxv4kwxithcfbhmgia.webp)

To save my face, I started debugging. I found out that he needs to upgrade the software on his computer to be able to use the new features. I helped him download and configure what was needed. Then the project worked.

Now Imagine I sent the project to ten people. I would have to help each of them set up and reconfigure their computer in order to test the new features. This would be stressful.

Only if there was a way to package everything needed to work on the application into one place, then share it with my collogues. They would download the package and run it. And the setup is complete. And the setup is complete.

Docker helps to solve this problem. This article explains **basic Docker concepts** and **shows you how to create containers for your applications.**

## What are Containers

![1_fbomrwph2esddv7mouq8hw.webp](/assets/img/virtualisation/tempo/1_fbomrwph2esddv7mouq8hw.webp)

A container is an imaginary box or package that contains everything needed to run your project. Things like

- the projects source code
- the project dependencies
- the config file (e.g .env files e.t.c)
- the database setup
- the runtime environment e.t.c.

A Container allows you to share and run your project without worrying about the system configurations needed to run the project.

> **Docker is a software that helps you create and manage containers.**

To create a docker container, you need to run a **docker image**.

## Docker image
A docker image defines the structure of a container. It serves as a template or blueprint for creating a docker container. With a single docker image, you can create many containers for your project.

> It’s like when you want to build a house. An architect draws out a floor plan for the house. With that floor plan, you can build many houses.
> 
> A **docker image** is the floor plan. Using a single docker image, you can create **many containers**.

The most popular way of creating a docker image is through a **docker file**.

## Docker File
A docker file is a text file that holds the instructions on how Docker will build your docker image. To build a docker image, Docker executes the commands written in the docker file line by line.

This is an example of a **Dockerfile**. This dockerfile is for a java application.

![1_ybeuaxt4ywqcduoujbs0fw.webp](/assets/img/virtualisation/tempo/1_ybeuaxt4ywqcduoujbs0fw.webp)

Let’s explain what the statement on each line does.

**Line 1** : `FROM openjdk:11`

Every docker image is built on an existing image. This existing image is called the **base image**. It can be

- an image of a runtime environment e.g java, node,
- or an image of an operating system e.g ubuntu.

The first instruction of every docker file must specify the base image.

The syntax for specifying the base image is

```docker
FROM <baseImageName>:<tag>
```

Docker will pull this image from a **docker registry** when it is trying to build your image. A docker registry is like GitHub. It is a repository of docker images. The most used docker registry is [docker hub](https://hub.docker.com/).

In the example above, the docker image is for a java application, so the base image used is a java runtime environment. For a node js project, you can use the image of node runtime environment as your base image.

**The tag** specifies the version of your base image. The example above uses the tag for java 11. You can specify a base image without the column and the tag. When you don’t specify a tag, Docker will download the latest version of the base image.

**Line 2**: `ADD target/*.jar app.jar`

A container contains everything needed to run the application. So to create a container, you need to copy the things needed to run the application into the docker image. This is done using either the `ADD` or `COPY` command. The example above uses the `ADD` command.

The example above is for a java application. So everything needed to run the application is already bundled into a jar file. You just need to copy the jar file into the container.

For other cases where you need the source code to run the app, you can copy the source code into the container. Here is an example.

![1_wd-ud1j3qm1bk5x_hyxgdg.webp](/assets/img/virtualisation/tempo/1_wd-ud1j3qm1bk5x_hyxgdg.webp)

In this node project example, you need both the source code and its dependencies to run the app. Line 2 makes a new directory. Line 3 copies the source code into the new directory. Then line 4 installs the dependencies into the container.

**Line 3**: `ENTRYPOINT [“java”,”-jar”,”/calculator.jar”]`

Every docker file must include the command that will run when you run a container. This command is usually the command used to start your application. For java applications, the `java -jar appname.jar` command is usually used. The command is specified using either the `ENTRYPOINT` or `CMD`. Every docker must have an Entry point command or `CMD` command.

When your docker file is ready, you can then containerize your project.

## How to contanazire your application

![1_vw_mai9ppqfzosk-spwkqq.webp](/assets/img/virtualisation/tempo/1_vw_mai9ppqfzosk-spwkqq.webp)

**Step 1: Create your application**
**Step 2: Download and install [docker](https://docs.docker.com/desktop/install/mac-install/)**
**Step 3: Build your docker image**

To build your docker image, you need a **Dockerfile**. In the root directory of your project, create a new file called `Dockerfile`. Define the structure of your docker image in the file, then run the docker build command.

![1_hot-p7l8fe7ltaskdmi14q.webp](/assets/img/virtualisation/tempo/1_hot-p7l8fe7ltaskdmi14q.webp)

- The `-t` flag is used to give the image a name and a tag.
- Ideally, your docker file should be located in the root directory of your project. So to specify its path, use `.`.

Your first build might take some time. This is because docker will have to download the base image from docker hub. This download happens only once. Later builds will be faster.

After building your image, you need to run a container from the image

**Step 4: Run your container**

To run a container, run the following command in the terminal of the root directory of your project.

![1_74bcmrlcdmghbkpmpdrulw.webp](/assets/img/virtualisation/tempo/1_74bcmrlcdmghbkpmpdrulw.webp)

> Use the `--name` flag to give a name to your container.

The docker run command creates a new container from the docker image. If you don’t specify a name for your container, docker gives your container a random name.

After you execute the run command, you should see your calculator running in the terminal.

Every time you use the docker run command, docker creates a new container.

- To restart an existing container, run `docker start containerName`
- To stop your running container, run `docker stop contanierName`
- To check the list of running containers, run `docker ps`.
- To get all containers, both running and not running, run `docker ps -a`.

Whenever you make an update to your code, you will need to rebuild your docker image.

## Sharing your docker image
You can share your docker image by sending your colleagues your docker file. When they get the docker file, they would run the respective docker build commands to get the docker image up and running.

You can also share your docker image through docker hub. Push your docker image to your docker repository then your colleagues can pull it and build containers from the image. The process is like how you share your code through GitHub.

## Conclusion
Docker is software for creating and managing containers. Containers make it easy to share and test your project.

To create a container for your project, use a docker file to build a docker image. This image should contain everything needed to run your project. Then use the image to create as many containers as you want.

Source : https://awstip.com/