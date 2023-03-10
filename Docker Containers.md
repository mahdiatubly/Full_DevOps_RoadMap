# Docker Containers

In this file, you will get a full guide to help you master Docker Containers and a full cheat sheet for all the important Docker commands and solutions to some of the bugs that may encounter you on the way.

## Where I Should Start?

If you want to know what containers are and why they are essential in the process of software development then this [course](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=4210s) is an excellent point to start. Also, you can read the first three chapters of Containerize your Apps with Docker and Kubernetes book which are published for free, the chapters are uploaded on the Full_DevOps_RoadMap repo.

## The Most Important Commands

- To run Docker CLI on a server and the docker engine on another use the following command:

        # docker -H=[IP Addr.]:2375 [the other command]
        $ docker -H=123.2.3.4:2375 run nginx

- Namespace system in linux is the responsible to make containers as separate spaces from the host OS and from each others. Cgroup is the resposible to manage resources of the server in a fair customizable way. You can set limit the container usage of the resouces as following:

        $ docker run --cpus=.5 ubuntu
        $ docker run --memory=.5 ubuntu

- Docker saves data in `var/lib/docker`

- To list all containers:

        $ docker ps
        # To include stoped containers in the result:
        $ docker ps -a

- To get all the saved images:

        $ docker images
        $ docker images | wc -l

_You can pipe the result to wc -l command to get the number of images or containers as in the prevoius example_

- To run a container with a specific image:

        # You can give the container a name with --name
        # You can the container in the background with -d which stands for deamon
        # -it used to run interactive mode (if the program require inputs)
        # docker container(optional) -d --name [container name] [image used to create the container] [you can a process to run on the container]
        # To assign an environment variable in the running process use -e as in the second example.
        $ docker run -d --name first-container nginx
        # -p [port # on the host]:[port number on the container]
        $ docker container run --name first-container -e lesson=docker -p 38282:8080 nginx
        # If you don't want to download the image you can just pull the image
        $ docker pull nginx

- To add an env. var. to code:

                >> x = os.environ.get(X)
                => then in docker
                $ docker run -e X=5000 first-image

- As a default Docker uses the latest version of the image, if you want to use a specific version you can use tags:

        # docker run [image name]: [the verion]
        $ docker run redis: 4.0

- The name flag is added to the container's running command mostly to be used in creating links between different containers.

- To cancel deamon property of a running container:

        # docker attach [container name or id]
        $ docker attach first-container

- To stop a container:

        # docker stop [container name]
        $ docker stop first-container
        # To stop all containers all at once (xargs: a Linux utility that tackes the input and excute the command on each input entry)
        $ docker ps | xargs docker stop

- To remove a container:

        # docker rm [container name or id]
        $ docker rm first-container
        # To remove all containers all at once
        $ docker ps -a | xargs docker stop

- To execute a process in an exist container:

        # docker exec [container name] [the process]
        $ docker exec first-container echo "Hello World!"

- To remove an image (You have to remove all the containers built on this image before):

        # docker image rm [image name or id]
        $ docker image rm nginx
        # To remove all containers all at once
        $ docker images | xargs docker image rm
        # Or
        $ docker image rm $(docker images)

_Instead removing all the images you can filter the result using grep command_

- To get the container's information:

        # docker inspect [contaienr name or id]
        $ docker inspect first-container

- To create an image you have to create a Dockerfile in the project directory, this file contains the required instruction to run the application. Following is an example on dockerfile:

        # The base OS or image of the new image.
        FROM Ubuntu

        RUN apt-get update
        RUN apt-get install python

        RUN pip install Flask
        RUN pip install Flask-mysql

        # Copying the program code.
        COPY ./code
        # command defines the action that will run when the container directly created.
        # If you try to update this command within the run statement the new value will
        # replace the exist value while with ENTRYPOINT the given value will be appended
        # to the exist value.
        ENTRYPOINT["sleep"]
        # Setting defaut value for ENTRYPOINT to avoid getting errors.
        CMD["5"]

Then, run:

        # docker build Dockerfile(mostly you need to write the path to the file without the file name) -t [image name]
        $ docker build Dockerfile -t first-image
        $ docker run first-image 1000

- To push the image to the docker registry (you may need to login before running this command):

        # docker login
        # docker push [your account name on docker hub]/[image name]
        $ docker push first-image

- To get information about the base OS of an image:

        # docker run [image name] cat /etc/*release*
        $ docker run python:3.6 cat /etc/*release*

- To get the container logs:

        # docker container(optional) logs [container name or id]
        $ docker container logs first-container

- To make the data of the containers persistant:

        # First create a volume (optional):
        $ docker volume create data_volume
        # Then run: docker run -v [volume name or directory on the host]: [the default saving location in the container ex: var/lib/mysql] mysql
        $ docker run -v data_volume: var/lib/mysql mysql
        ## The modern way: docker run --mount type=bind [or volume],
        source=[location on the host] target=[location on the container] mysql
        $ docker run --mount type=bind, source=/home, target=var/lib/mysql mysql

- To run an application that distributed through multiple containers, then you need to create a YAML file. YAML is a data format similar to JASON and XML,It will run all of the app services and link them to each other. Following is the structure of the YAML file:

        version: 2
        services:
                [The container name]:
                image: [base image of the container] (if you did not create the the image you can just use instead of image {build: [the project path that contains the Dockerfile] )
                ports:
                        - [#:#]
                environement:
                        POSTGRES_USER: postgres
                        POSTGRES_PASSWORD: postgres
                depends_on:
                        - [the name of the base container]
                networks:
                        - [backend]
                ...
                ..
                .
        networks:
                [ex: front-end]:
                [ex: backend]:

- Some rules of YAML format:

```
        - key value pairs => key:[space] value /: Fruite: Apple
        - List => Fruits:
                  - Apple
                  - Orange
                  - ...
        - Ditionary => Apple:
                       [space]  color: green
                                Origin: Jordan
                        Orange:
                                color: orange
                                origin: Jordan
        - Comments => # The comment
```

- After creating the compose file in YAML format, run it through the following command (this will compose the app):

        # You can add -d command to run containers in the deamon mode.
        $ docker-compose up

- Docker has 3 default networks:

  1.  Bridge Networks: it's an internal private network created by the docker host. Each container gets an IP address in the range of 127.17.0.0, containers can use these addresses to connect to each other. To access the containers externaly you have either to associate ports on containers to ports on the host docker or to use host network.
  2.  Host Network: is the network of the host, you can use it to run containers and then you do not need map ports but you can't have two containers running on the same port. To run a container on the host network:

                $ docker run nginx --network=[host address]

  3.  None: The container hasn't any connection to any network

- You can create your own custom network.
- To add a tag to an image to push to local host:
        # docker tag SOURCE_IMAGE TARGET_IMAGE[:TAG]
        $ docker image tag httpd:latest localhost:5000/httpd:latest
