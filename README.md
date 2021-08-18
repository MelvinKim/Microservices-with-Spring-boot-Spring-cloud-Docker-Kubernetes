# Microservices-with-Spring-boot-Spring-cloud-Dokcker-Kubernetes
This repo demonstrates how to create microservices using Spring boot, Spring cloud, Docker, Docker Compose, Kubernetes, Zipkin etc


# Building Microservices with spring boot 
# Microservices: 
Small autonomous services that work together, communicate via REST
Exposed by REST
Small well chosen deployable Units
Cloud Enabled
You can have multiple instances of each microservice

# Challenges with microservices: 
Boundary context
Configuration management
Solved by spring cloud config server
Dynamic scale up and scale down
Solved by Eureka(registering and discovering microservices)  server, Ribbon(Client Side Load Balancing), Feign
Identifying bugs in microservices, in which microservice did the bug occur? ...logging can help here
Solved by Zipkin Distributed Tracing, Netflix API Gateway
Fault Tolerance
Solved by Hystrix


# Advantages of Microservices: 
Easy adaptation to new technologies and processes
Dynamic scaling
Faster release cycles, because you are developing smaller components


# How to configure properties using “application.properties” file 
Type the properties in the “application.properties” file
Create a configuration class; do not forget the two annotations
Inject the Configuration class in the controller; do not forget to Autowire it


