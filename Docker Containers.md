# Docker Containers

In this file, you will get a full guide to help you master Docker Containers and a full cheat sheet for all the important Docker commands and solutions to some of the bugs that may encounter you on the way.

## Where I Should Start?

If you want to know what containers are and why they are essential in the process of software development then this [course](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=4210s) is an excellent point to start. Also, you can read the first three chapters of Containerize your Apps with Docker and Kubernetes book which are published for free, the chapters are uploaded on the Full_DevOps_RoadMap repo.

## The Most Important Commands

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
        $ docker container run --name first-container -e lesson=docker -p 38282:8080 nginx
        # If you don't want to download the image you can just pull the image
        $ docker pull nginx

*As a default Docker uses the latest version of the image, if you want to use a specific version you can use tags:*

        # docker run [image name]: [the verion]
        $ docker run redis: 4.0

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

- To create an image you have to create a Dockerfile in the project directory, this file will contain the required instruction to run the application. Then, run:

        # docker build Dockerfile(mostly you need to write the path to the file without the file name) -t [image name]
        $ docker build Dockerfile -t first-image

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

        # docker run -v [location on the server]: var/lib/mysql mysql
        $ docker run -v Downlads/dataDirect: var/lib/mysql mysql

- To run an application that distributed through multiple containers, then you need to create a yaml file. It will run all of the app services and lik them to each other. Following is the structure of the yaml file:

        [The container name]:
            image: [base image of the container] (if you did not create the the image you can just use instead of image {build: [the project path that contains the Dockerfile] )
            ports:
                - [#:#]
            links:
                - [the name of the linked container]

- After creating the yaml file, run it through the following command (this will compose the app):

        docker-compose up
