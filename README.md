# Architectural diagram
<img src="https://res.cloudinary.com/dyjp6vejk/image/upload/v1629285029/sample-pic_itupeu.png" />


# Microservices-with-Spring-boot-Spring-cloud-Dokcker-Kubernetes
This repo demonstrates how to create microservices using:
- Spring boot
-  Spring cloud
-  Docker
-  Docker Compose
-  Kubernetes
-  Zipkin 
-  rabbitMq


# Building Microservices with spring boot 
# Microservices: 
- Small autonomous services that work together, communicate via REST
- Exposed by REST
- Small well chosen deployable Units
- Cloud Enabled
- You can have multiple instances of each microservice

# Challenges with microservices: 
- Boundary context
- Configuration management
- Solved by spring cloud config server
- Dynamic scale up and scale down
- Solved by Eureka(registering and discovering microservices)  server, Ribbon(Client Side Load Balancing), Feign
- Identifying bugs in microservices, in which microservice did the bug occur? ...logging can help here
- Solved by Zipkin Distributed Tracing, Netflix API Gateway
- Fault Tolerance
- Solved by Hystrix


# Advantages of Microservices: 
- Easy adaptation to new technologies and processes
- Dynamic scaling
- Faster release cycles, because you are developing smaller components


# How to configure properties using “application.properties” file 
- Type the properties in the “application.properties” file
- Create a configuration class; do not forget the two annotations
- Inject the Configuration class in the controller; do not forget to Autowire it

## Docker
Docker:
- self contained unit of software
- contains everything required to run code eg code, OS, networking required, processes, dependencies

# Docker Flow: 
Image = docker run  =>  Running container  ⇒ stopped container  =  docker commit ⇒ new-image ==  docker tag “image-ID” “new-name”  => give the new image a new name
**Images => fixed points
**docker container exit codes

Running Things in docker
- running a container but you don’t want to keep it afterwards, deleting the container once it exits
 - docker run --rm -ti ubuntu 
- to make a container sleep for a certain duration
  - docker run --rm -ti ubuntu sleep 5
 **docker run -ti bash -c "sleep 3; echo all done"
1. running stuff in detached mode
 - docker run -d -ti ubuntu bash
2. running stuff to be attached to the Terminal
 - docker attach “name of the container”
3. detach but keep container running
 - Ctrl + p, Ctrl + q
4. container is running, but you want to add another process to the running container
- docker exec -ti  nervous_zhukovsky bash
5. to log container output
 - docker logs “container_name”
6. To kill a container
 - docker kill “container-name”
7. to remove a container
 - docker rm “container_name”

- NB:To limit memory that a container can take
 - docker run --memory “maximum-allowed-memory” “image-name” command
- NB:To limit CPU time that a container can take
 - docker run --cpu-shares relative to other containers
 - docker run --cpu-quota to limit it in general

Exposing Container ports








**Nb:
1. To list all docker images
 - docker images
- ti => terminal interactive i.e
- docker run -ti
2. to run the latest image
 - docker run -ti ubuntu:latest
2. to list running containers
 - docker ps
3. to commit a new image
 - docker commit “original name” “new-name”
4. to list all containers
 - docker ps -all
5. To see the last exited container
 - docker ps -l
- Files created in one container, stay in that container. They don’t move to another container

Nb:
1. to list stuff in Linux:
 ls
2. To create a blank file in linux
 touch

Tagging new images structure
=>registry.example.com:port/organization/image-name:version-tag

Cleaning up images.
-docker rmi image-name:tag
-docker rmi image-id

# Docker volumes:
-Types of docker volumes
1. persistent
2. Ephemeral - exist as long as the container is using them

**Docker Architecture**
    Docker client
        |
    Docker Daemon
        |
Containers    Local-images    Image Registry
Docker client => This is where we run our commands
Docker Daemon | Docker Engine => This is where the commands are executed
    -Responsible for: 
        i)Managing containers
        ii)Managing local images
        iii)Image Registry

    Container
        |
    Docker Engine
        |
    Host Operating System
|
Cloud Infrastructure

# Playing around with Docker Images    
1. Listing all available images
docker images
2. Tagging images
    docker tag repo-name:tag  (you can even give more than one tag to an image)  eg docker tag repo-name:original-tag  docker tag repo-name:new-tag 

3. Pulling an image from docker hub: (just downloads the images from docker hub, does not run it) 
docker pull image-name 
4. To download and run use : 
    docker run image-name
(first checks if the image is available in the local repo, if not it downloads it from docker hub)
5. To Search images, use 
    docker search image-name
6. To look at the history of an image: (shows steps involved in creating an image)
    docker image history image-id or
    docker image history repo-name:tag
7. To inspect an image
    docker image inspect image-id
8. To remove an image from the local repo
    docker image remove image-Id

# Playing with docker containers 
1. To list  running containers: 
    docker container ls
2. To list all containers
    docker container ls -a
3. To create a docker container: 
    run  the image i.e
    - docker run -p host-port-number:docker-port repo-name:tag eg
    - docker run -p 5000:5000 -d  in28minutes/todo-rest-api:latest or 
    - docker run -p 8000:8000 -d melvinkimathi/spring-boot-app:1.0.0.RELEASE
4. To pause a container: 
    docker container pause image-ID
5. To unpause a specific container: 
    docker container unpause image-ID
6. To display logs of a specific container: 
    docker logs -f image-ID 
7. To stop a running container --- gracefully shuts down a container
    docker container stop image-ID 
8. To inspect a container
    docker container inspect image-ID  
9. To remove all stopped containers
    docker container prune
10. To kill a running container: 
    docker container kill image-ID
11. To create a restart policy 
    (if set to always, if docker desktop starts the container automatically restarts)
    (very useful when dealing with things like databases) - to ensure they are always up and running
    - docker run -p host-port:container-port -d --restart=always repo-name:tag eg 
    - docker run -p 5000:5000 -d --restart=always in28min/todo-rest-api-h2:1.0.0.1
    - docker run -p 5000:5000 -d --restart=no in28min/todo-rest-api-h2:1.0.0.1
NB: 
**The difference between docker stop and docker kill is that, docker stop gracefully shuts-down a container (allows running processes to terminate successfully), while docker kill stops the container immediately**
- To list all events happening with Docker: 
docker events
- To show all starts regarding a specific container
docker stats image-ID
- To specify the memory  and cpu limits in your containers:
docker run -p 5000:5000 -d -m 512m --cpu-quota 5000 repo-name:tag
-m (memory limit)
--cpu (cpu quota limit) 
5000/100000 = 5%
- To check the top process running in a specific container: 
docker top image-ID
- Lists all the different things being managed by Docker Daemon
docker system df

        


# Distributed Tracing  
-Microservices have a **complex call chain**
-**How do you debug problems?**
-**How do you trace requests across microservices?**
-Go for Distributed Tracing
⇒ How does this work? 
-All the microservices involved would actually send all the information out to a distributed tracing server
-the distributed tracing server would store everything in a database...can be an in-memory DB or a real DB eg mySQL. In production environment it’s advisable to use a real DB
-Zipkin can be launched as a Docker container
⇒ Launching zipkin using Docker: 
    docker run -p 9411:9411 openzipkin/zipkin:version-you-want
-After launching it, add a set of dependencies in the porm.xml file
-**NB:
    -To be able to trace requests across microservices, each request should be assigned a unique ID, that where sleuth comes into play

How spring boot apps in docker 
1. Add some configuration in the maven plugin found in the porm.xml
2. Run the app, as a maven build, add this to pop-out dialog “spring-boot:build-image -DskipTests”
3. Then click on “run”

# How to start your apps using Docker compose
1. Create a yaml file
2. Input your declarations
3. Then cd into that folder containing the yaml file and type
4. docker compose up
    
        









