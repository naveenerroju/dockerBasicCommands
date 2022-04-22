Docker 

Docker is all about speed
Build, Ship/share and Run

Images

Images are package or template such as VM Template
Images are used to create one or more containers
Images are read only template used to create containers
Images are stored in the Docker registry (Docker Hub)

________________________________________________________________________________________
Containers

Containers are running instance of the images
Containers are isolated and have their own environment 
Lightweight

Containers are created from images. 
It has all the binaries and dependencies needed to run the application

Containers are serverless
Containers designed to run stateless apps meaning which not to store any data but nowadays containers are running as a stateful application for storing data

Container will package all your applications code and its dependencies and ship it to either public or private repositories and then download it and run it anywhere.
_______________________________________________________________________________________
Orchestration

What if your application relies on other containers such as database or messageing services or web server or other backend services?

What if the number of users increase and you need to scale your application?

You would like to scale down when the load decreases

To enable these functionalities you need an underlying platform 

The platform needs to orchestrate the connectivity between the container and automatically scale up and scale down based on the load

This whole process of automatically deploying and managing container is known as Container Orchestration
_________________________________________________________________________________________

Registries and Repositories

Registry where store images
You can host your own registry
Or
You can use Docker's public registry which is called DockerHub

Inside the registry, images are stored in repositories

Docker Hub : Example Google drive is Docker Hub
Docker Images : Example google drive, files are docker images
Docker Repository : Example google drive, folders are docker repository

Lab 1)

1)	Download images from Docker Hub
2	Go to Docker Hub website and register it
3)	At terminal, apply 'docker pull centos'
	Ensure that centos images download or not
	docker images
4)	Apply, Docker run command as
	docker run -it -d  --name mycentos <Image Name> or <Image Id>
	Example
	docker run -it -d  --name mycentos centos 
	(centos is a repository and -it means interactive and -d detached)
	docker run commands allow images into container and image become active now
5)	Apply docker ps -a (Display all active and inactive process details)
6)	Apply, docker exec -it <<Container Id>> /bin/sh
	or
	docker exec -it <<Container Id>> bash
7)	Spin down to new terminal
8)	At new container os <Peform Instllations as per discussions>
9)	Apply exit command (exit from container)
	Apply docker login (provide Docker hub username and password)
10)	Apply docker commit <<Container id>>  <<username / new image name>>:<<tag name>>
	docker commit ced605ecbbae nramnad/myimage:1
	(Docker commit for Saving changes to a container)
	(Commit for allowing own images)
11) 	New images get display, when you apply docker images command
	docker images
12)	Upload newly created images to Docker hub
	docker push <<docker hub user name / Image Name>>:<Tag Name?

___________________________________________________________________________________________
Lab 2

Apply below commands
This is for downloading Neginx WebServer
(docker run is old way and docker container run is new way)

docker container run --publish 80:80 nginx

Go to browser and public DNS value where you get Nginx server welcome page

At terminal, you get backend process details are displaying

In order to get out of backend process displaly, apply below commands

docker container run --publish 80:80 --detach nginx


docker ps
or 
docker container ls

How to stop the container

docker container stop << Container Id >>

docker container ls

docker container ls -a

Docker Run Vs Start

'docker container run' always starts a *new* container

Use 'docker container start' to start an existing stopped one

Another way to generate container

docker container run --publish 80:80 --detach --name webhost nginx

docker container logs webhost

docker logs webhost (oldway)

docker container top webhost
(Display the running process of a container)

docker container rm
(Delete container)




_________________________________________________________________________________


Tips

How to remove entire PS files
docker rm $(docker ps -a -f status=exited -q)

Lab 3)

Docker File

Docker file can be used to automate the creation of a Docker Container Image
Docker file is a recipe for how to build the container image
Blueprint for building images
Build your own images
Basically definition of Docker images
Use certain instructions for building images
Going from file to the images this process is called building

1) Create new folder at Ubuntu
2) Create new file name as a Dockerfile (Please note 'D' should be uppercase)
3) Apply simple Docker command for creating dockerfile
4) Code should be like this
	FROM ubuntu:14.04
	MAINTAINER nramnad@hotmail.com
	RUN sudo apt-get update
	RUN sudo apt-get install apache2 -y
	RUN sudo apt-get install git -y
	RUN sudo apt-get install tomcat7 -y
	RUN sudo apt-get install python -y
	RUN sudo apt-get install nano -y
	RUN sudo apt-get install vim -y

	
5) Execute docker file commands as follows
	docker build -t <Docker Hub User Name>/<Your own Image Name>:<Tag Name> .
	Example: docker build -t nramnad/myubuntu:v1 .
	sudo is an optional unless until you are not a root user
	nramnad is a Dockerhub username
	myubuntu is a image name
	v1 is tag name
	. denote for current folder other you need to mention required folder details

6) Apply docker images command which indicates that whether images created or not

7) Create a container based on new image 

8) After get into container and ensure that below these applications has installed or not

apache2 
git
python
nano
vim

How do you know whether Tomcat installed or not
grep "Apache Tomcat Version"
**************************************************************************************************************************

Step 3:
Java installation

FROM ubuntu:16.04
MAINTAINER nramnad@hotmail.com
# Install OpenJDK-8
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get install -y ant && \
    apt-get clean;

# Fix certificate issues
RUN apt-get update && \
    apt-get install ca-certificates-java && \
    apt-get clean && \
    update-ca-certificates -f;

# Setup JAVA_HOME -- useful for docker commandline
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME