---
title: Docker for Dummies
---

# Docker Overview

## Why do you need Docker?

-   Compatibility and Dependency

-   Long setup time (A new member expend a lot of time creating the
    setup)

-   Different Dev/Test/Prod Environments

<img src=".//media/image1.png" style="width:6.5in;height:4.64306in" alt="Diagram Description automatically generated" />

## What can it do?

-   Containerize Application

-   Run each service with its own dependencies in separate containers

<img src=".//media/image2.png" style="width:6.5in;height:5.09028in" alt="Graphical user interface, application Description automatically generated" />

## What are containers?

-   Completely isolated environments (can have their own processes,
    networks)

-   Like VM but they all shared the same kernel.

-   Based on LXC containers

<img src=".//media/image3.png" style="width:6.5in;height:3.37361in" alt="Graphical user interface Description automatically generated" />

## Operating System

-   All consist in an OS Kernel and a set of software’s (UI, Drivers,
    file managers, etc.)

-   The Kernel its responsible of interacting with the underlying
    hardware

<img src=".//media/image4.png" style="width:6.5in;height:2.61597in" alt="Graphical user interface, application Description automatically generated" />

## Containers vs Virtual Machines

<img src=".//media/image5.png" style="width:6.5in;height:3.93264in" alt="Graphical user interface, application Description automatically generated" />

<img src=".//media/image6.png" style="width:6.5in;height:4.03611in" alt="Graphical user interface Description automatically generated" />

## How is it done?

<img src=".//media/image7.png" style="width:6.5in;height:3.20347in" alt="Graphical user interface Description automatically generated" />

## Container vs image

-   An image: is package or template and its use to create one or more
    containers

-   Containers: Running instances of images that are isolated

<img src=".//media/image8.png" style="width:5.89705in;height:4.88333in" alt="Diagram Description automatically generated" />

# Docker commands

| **Command**                          | **Function**                                                                                                                                                                                                                  |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| docker run (-d) *image_name*         | Run an instance of image_name_app if the image it’s not present then will be download from the docker hub. (The same as before but running on the detach mode, this will run in the background, and you can use the terminal) |
| docker ps (-a)                       | List of containers (all running or not containers)                                                                                                                                                                            |
| docker stop *id* or *container_name* | Stop a container                                                                                                                                                                                                              |
| docker rm *id* or *container_name*   | Remove a container, the correct output is the name of the container or id. It’s not necessary to provide the full id, and you can remove more than one by writing the ids (a part) separated by space.                        |
| docker images                        | List of available images                                                                                                                                                                                                      |
| docker rmi *image_name*              | Remove images (All containers must be deleted!)                                                                                                                                                                               |
| docker pull *image_name*             | Download (not run) an image                                                                                                                                                                                                   |
| docker exec *container_id command*   | Execute a command on a specified container                                                                                                                                                                                    |
| docker inspect *container_name*      | Details of the container in JSON format                                                                                                                                                                                       |
| docker logs *container_name (or id)* | Logs of detach running container                                                                                                                                                                                              |

*Containers are meant to run a specific task or process such as to host
an instance of a web server, application server or a database, etc.*

*Once the task its complete the container exits*

*A container only lives as long as the process inside it is alive.*

<img src=".//media/image9.png" style="width:6.5in;height:1.6in" alt="A screenshot of a computer Description automatically generated with medium confidence" />

This is why when you run a container from an ubuntu image it stops
immediately because ubuntu is just an image of an OS, there is no
process or app running by default.

# docker run

-   Tag:

    -   Uses to specify the version of the image, latest by default.

    -   docker run *image_name:**version***

<img src=".//media/image10.png" style="width:6.5in;height:2.95069in" alt="A screenshot of a computer Description automatically generated with medium confidence" />

-   STDIN:

    -   By default, the docker container does not listen to a standard
        input, does not have a terminal.

    -   docker run **-i** *image_name* (interactive mode)

    -   docker run **-it** *image_name* (interactive mode + pseudo
        terminal)

-   PORT mapping (Web apps):

    -   Every docker container has an IP (internal) by default but its
        only accessible within the docker host.

    -   We can map a port (e.g. 80) of local host to port (e.g. 5000) on
        the docker container using **-p** in run command.

    -   docker run **-p** host_port:container_port *image_name*

-   Volume mapping:

    -   If you want that the data persist, you shouldn’t save the data
        in the container since once its deleted it will disappear.

    -   We can map a folder (e.g. /opt/datadir) of the computer and map
        that to a container folder (e.g. /var/lib/mysql), using **-v**
        command.

    -   docker run **-v** /opt/datadir: /var/lib/mysql *image_name*

<img src=".//media/image11.png" style="width:6.225in;height:2.68154in" alt="Graphical user interface Description automatically generated" />

# Jenkins tutorial

Go to <https://hub.docker.com/_/jenkins> if you want more details.

We are going to explore the run command using de **-p** and **-v**
options by following the next steps:

-   Start Docker Desktop:

> <img src=".//media/image12.png" style="width:5.6971in;height:4.42256in" alt="Graphical user interface, application Description automatically generated" />

-   Check that its running by open a CMD terminal and execute:

    -   docker –version

<img src=".//media/image13.png" style="width:5.69583in;height:1.79271in" alt="Text Description automatically generated" />

-   Run (or pull) the Jenkins image:

    -   docker run jenkins/jenkins

<img src=".//media/image14.png" style="width:5.9634in;height:3.13333in" alt="Text Description automatically generated" />

-   This will download the latest image available (since we did not
    specify the version by using tag : )

-   Since we run the image on attach mode our console now is not
    available and shows the info of the running container.

> <img src=".//media/image15.png" style="width:6.11079in;height:4.28016in" alt="Text Description automatically generated" />

-   Open another terminal to see the container info by executing: docker
    ps

> <img src=".//media/image16.png" style="width:6.89249in;height:1in" alt="A screenshot of a computer Description automatically generated with medium confidence" />

-   We can only access to this container using the internal port (8080
    or 5000), since we didn’t map the container port to a local port, to
    do this:

    -   Retrieve more details of the container by executing: docker
        inspect *container_id*

    -   This will display a lot of information, but we are interested in
        the Networks part more exactly in the IP Address: 172.17.0.1

> <img src=".//media/image17.png" style="width:6.01629in;height:2.36667in" alt="Text Description automatically generated" />

-   Go to the Docker Desktop and open a terminal of the container:

<img src=".//media/image18.png" style="width:7.06451in;height:1.21667in" alt="Background pattern Description automatically generated" />

-   And execute: curl 172.17.0.2:8080, this will show the html code of
    the web page.

> <img src=".//media/image19.png" style="width:6.9889in;height:1.81667in" alt="Text Description automatically generated" />

-   To be able to make this connection using our browser we need to map
    the container port to a local host port. To do this:

    -   Stop the current container and delete the image (the order
        matters!):

        1.  docker stop container_id

        2.  docker rmi jenkins/jenkins

        3.  Remark: this step is not necessary it’s just to keep our
            environment clean.

        4.  Create the image again but now using the **-p** option to
            map the port: docker run -p 8080:8080 jenkins/Jenkins

-   When it finishes, you can access to the web app using your computer
    navigator: localhost:8080

> <img src=".//media/image20.png" style="width:6.17156in;height:3.36667in" alt="Graphical user interface, text, application Description automatically generated" />

-   The password it’s at the end of the execution:

<img src=".//media/image21.png" style="width:6.5633in;height:1.8in" alt="Text Description automatically generated" />

-   The next step is customizing the web app by downloading the plugins,
    select the default option.

> <img src=".//media/image22.png" style="width:5.6725in;height:3.64167in" alt="Graphical user interface Description automatically generated" />

-   When the installation finish (takes several minutes) we can create
    an Administrator user:

> <img src=".//media/image23.png" style="width:5.11667in;height:4.79086in" alt="Graphical user interface, application Description automatically generated" />

-   This is the web app interface for the Administrator user

<img src=".//media/image24.png" style="width:6.50208in;height:2.89537in" alt="Graphical user interface, application Description automatically generated" />

-   The problem with this run execution is that once we stopped the
    container all the data will be destroy, so we’ll need to install
    again all the plugins and create the user every time we create
    another container based on the jenkins/jenkins image.

-   To solve/avoid this problem we use the **-v** option to map a local
    directory

    -   Stop the current container

    -   Execute: docker run -p 8080:8080 **-v** /local_folder:
        /var/jenkins_home jenkins/jenkins

    -   Repeat all the previous steps (download plugins, create admin
        user)

    -   Stop and run again the container to see that this time your data
        will be there.

# Docker Images

You will create an image because you cannot find a component or a
service that you need as a part of your application on docker hub, or
you want to dockerize your application.

## How to create an image? (Based on Python Flask)

Manually you will usually need to follow the followings steps:

1.  OS (Ubuntu)

2.  Update apt repo

3.  Install dependencies using apt

4.  Install Python Dependencies

5.  Copy source code to /opt folder

6.  Run the web server using ‘flask’ command

With Docker you create a docker file that does the job.

## Dockerfile

A dockerfile its simple txt file composed of **Instructions** and
**Arguments**.

<img src=".//media/image25.png" style="width:6.5in;height:3.26944in" alt="A screenshot of a computer Description automatically generated with medium confidence" />

## Layered architecture

-   When dockers create an image, it builds by layers.

-   Each line of instruction creates a new layer in the docker image
    with just the changes of the previous

-   All the layers build are cached this helps to restart a docker build
    from a particular step or when you want to add new layers.

> <img src=".//media/image26.png" style="width:6.93145in;height:2.04167in" alt="Text Description automatically generated with medium confidence" />

-   You can access to this info executing:

<!-- -->

    docker history image_name

## Environment Variables

We can pass an ENVIRONMENT VARIABLE to the container we use the **-e**
command

docker run **-e** VAR_NAME=var_value *image_name*

For example, if we have an image (webapp_image) in this case we can pass
the env variable *APP_COLOR* executing:

docker run **-e** APP_COLOR=green *webapp_image*

<img src=".//media/image27.png" style="width:6.06335in;height:4.5423in" alt="Text Description automatically generated" />

If you want to find the environment variables of a running container,
you can use the **inspect** command.

docker **inspect** *container_name(id)*

For a more explicit output, run:

docker exec -it *container_name* **env**

## Commands and Entrypoint

Remember that containers are not meant to host an OS, are meant to run
task or process.

So who defines what process is run within the container?  
In the dockerfile there is an instruction called **CMD** (command) that
defines the program that will be executed within the container when it
starts.

For example this is a dockerfile for the [Nginx
image](https://hub.docker.com/_/nginx) so the image starts with *nginx*
command.

<img src=".//media/image28.png" style="width:5.225in;height:4.00472in" alt="Text, letter Description automatically generated" />

If we look at a dockerfile of the Ubuntu image, will see that it uses
*bash* as the default command, that is not a process it’s a shell the
listen from inputs, so that’s why a container of a plain ubuntu image
stops immediately after its instantiation.

<img src=".//media/image29.png" style="width:5.15833in;height:5.26635in" alt="Graphical user interface, text, application Description automatically generated" />

We can run a process and avoid that stops executing: docker run ubuntu
sleep 10 (or other instruction), but if you want to do this change
permanent you can create your image as follows:

<img src=".//media/image30.png" style="width:6.5in;height:3.39514in" alt="Graphical user interface, website Description automatically generated" />

In this example the 5 seconds are hardcoded, if we want to modify this,
we can run:

docker run ubuntu-sleeper sleep 10, this will overwrite the default CMD
sleep in the dockerfile.

But doesn’t make sense typing sleep command again, if we want to just
pass the value as a parameter, we need to use the **ENTRYPOINT**
instruction.

<img src=".//media/image31.png" style="width:6.5in;height:2.89375in" alt="Graphical user interface, text, application, chat or text message Description automatically generated" />

The **ENTRYPOINT** instruction it’s like the **CMD** instruction as in
you can specify the program that will run when the container starts, but
whatever you specify in the command line will be appended to
**ENTRYPOINT**.

But by this way it will be no default value for the **ENTRYPOINT,** so
we need to combine both **CMD** and **ENTRYPOINT** instructions:

<img src=".//media/image32.png" style="width:6.5in;height:1.53056in" alt="A screenshot of a computer Description automatically generated with medium confidence" />

**<span class="smallcaps">Difference:</span>** In the case of the
**CMD** instruction the command line parameters passed will get replaced
entirely whereas in case of **ENTRYPOINT** will get appended.
