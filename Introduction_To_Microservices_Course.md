# Introduction to Microservice Principles and Concepts

www.educative.com course.

## Introduction

**Microservices are independently deployable services**.

When modules are implemented, services can be changed independently. This cuts down on the number of tests and does away with the need to version an entire application.

To achieve this **every microservice has to be an independent process**. A good solution to decouple microservices is to use independent machines or Docker containers. Containers can be tested with other containers.

Advantages of microservices:

1. It is very compact
2. It is very general and covers a lot of situations
3. Modules are a well understood concept
4. Independent deployment gives many advantages

A **deployment monolith** is the deployment of all of the microservices at once.

## Reasons for using Microservices

1. Easy scalability of development

   1. Teams that are responsible for an individual microservice can make independent decisions about their service.
   2. When services are delivered as containers, each container only has to offer an interface to the other containers.
   3. The internal structure of a container does not matter.
   4. The service can be developed independently
   5. The service can be brought into production on its own

   Teams can act independently in their own domains. This minimizes the coordination effort.

2. Replacement of Legacy Systems

   1. Code is poorly structured
   2. Changes are not checked by tests
   3. Developers have to deal with outdated technologies

   Microservices can replace parts of the legacy system as you work to refactor it. In this case we use an entirely new codebase and we treat things like a greenfield project.

3. Sustainable Development

   Microservices promise long term sustainability.

   1. Replacability of microservices - When a service can no longer be maintained it can be rewritten or altered. To achieve replacability **dependencies between microservices have to be managed well**.

4. Dependencies have to be managed

   1. Classical architectures have difficulties at this level. Developers are often forced to introduce new dependencies. This often goes unnoticed until it is a problem. The more dependencies there are the more unstructured the system is.
   2. Microservices architectures do better
      1. There are clear boundaries due to their interface
      2. It is unlikely that architecture violations will occur because it will be immediately obvious that something has been violated.
      3. Interfaces between microservices can be considered **architecture firewalls**. 

Ideally, it would be impossible to introduce more dependencies between microservices. Thus **microservices ensure a high quality architecture**. Ideally this means development speed remains constant but we all know that isnt true.

## The Continuous Delivery Advantage

**Continuous delivery** is an approach where software is continuously brought into production with the help of a pipeline like this one:

`Commit -> Acceptance Tests -> Capacity Tests -> Explorative Tests -> Production`

Phases of continuous delivery:

1. Commit Phase
   - Software compilation
   - Unit tests
   - Static Code Analysis
2. Acceptance Test Phase
   - Automated tests assure correctness of software regarding domain logic
3. Capacity Test
   - Test performance under load
4. Explorative Test
   1. Test for things not covered in unit tests

Microservices represent independent modules and **generally have their own continuous delivery pipeline**.

Because the deployment units are smaller, the continuous delivery pipelines are significantly faster.

The tests are actually faster because there is less functionality per module.

**Building up a pipeline is easier for microservices**.

The deployment of a microservice poses smaller risk than monoliths.

### Deployment must be automated

Microservices significantly increase the number of deployable units and thus things better be automated in order to scale. Integration tests must be reduced to the minimum.

## More Advantages

1. Robustness - Microservices are more robust. **Resilience is the ability for a microservice architecture to keep running if one of the services fails**. Without resilience the availability of a microservice system might be a problem.
2. Independent Scaling - Most of the time, scaling the whole system is not required. **Each microservice can be independently scaled**. It is possible to start multiple instances of a microservice and distribute the load among them. This can improve the scalability of a system enough to handle instances of high load. **Microservices must be stateless**. Scaling can be very fine grained with a microservice.
3. Free Technology Choice - Each microservice can be implemented with an individual technology. This can facilitate migration to new technologies in the future as it makes no difference what tech the service is running as long as the interface is correct. Risk of choosing new technologies can be less risky by trying things in one microservice first.
4. Security - Blah Blah.
5. Isolation 
   1. Facilitates continous delivery.
   2. Improves robustness.
   3. Improves scalability
   4. Free tech choice
   5. Safeguarded by firewallys
   6. Architecture is rarely violated
   7. Ease of replacement
   8. Decoupling
      1. Easier to reason about
      2. Security easier to verify
      3. Performance easier to measure
      4. Easier to figure out correctness
      5. Design and development are easier

##Tradeoffs, Prioritizing, Advantages, and Levels

### Prioritizing Advantages

Different advantages are relevant when replacing a monolith with a microservice architecture.

1. Easier Scaling of Development - More pieces = more speed because more developers work on it
2. Easy Migration - Easier migration away from legacy deployment monolith
3. Continuous Delivery

At the end of the day it is about **increasing business value**.

### Microservices Involve Tradeoffs

Robustness:

- When robustness is the goal, services must be introduced in separate containers
- When robustness does not matter, other alternatives can be considered. Multiple microservices can run on one app server. They all run in one process on the server. Still independently deployable. Could be destroyed by things like memory leaks.

### Two Levels of microservices: Domain and Technical

There can be two main reasons to divide into microservices:

1. A course grained **division by domain** enables the teams to:

   1. Develop independently
   2. Allows them to roll out a new feature with the deployment of one microservice.

   Example: `Search | Check Out | Payment | Delivery`

2. Microservices can be further **divided for technical reasons**

   1. These can be scaled independently

   Example ` Search -> Full Text Search, || Category Based Search | Checkout | Payment ...`

## Challenges of Microservices

### Increased Operations Effort

The **operation** of a microservice system requires more effort than running a deployment monolith

1. More units have to be deployed and monitored.
2. Only feasible if operation is largely automated and ensurable by monitoring

### Must be Independently Deployable

Microservices must be independently deployable by deploying them to Docker or another container system.

Changes to interfaces must be implemented in such a way that an independent deployment of individual microservices is still possible.

- For instance it is good practice to offer the old and the new interface when a service is upgraded

### Testing must be independent

When all microservices have to be tested together, one microservice can block the test stage and prevent deployment of the other microservices making testing much harder.

**Testing has to be independent from both sides of the interface**.

### Difficult to change multiple microservices

Changing multiple microservices at once is more difficult than monoliths

- In a microservice system changing multiple things requires multiple deployments which also must be coordinated.
- In the case of a deployment monolith only one deployment would be necessary.

### Lost Overview

In a microservice **the overview of the system can get lost**. However, a sound system will usually only require changes to one or two microservices at a time. The high degree of independence waylays this.

### Weighing Benefits and Advantages

The most important rule is that **microservices should only be used if they represent the simplest solution**.

### Experiments

The following approach helps to find the right recipe to divide a system into microservices:

1. Identify the problems in your current system
2. Prioritize the benefits of using a microservice architecture
3. Weigh the challenges and risks
4. Look at the possibilities to determine the best fit.

## Microservices Chapter Conclusions

Microservices represent an extreme type of modularization. Their **separate deployment is the foundation of a very strong degree of decoupling.**

The **crucial benefit is isolation** at different levels.

- Facilitates deployments but also limits potential failures to individual microservices.
- Microservices can be individually scaled, technology decisions can be made independently, and security problems 
- can be restricted to individual microservices.
- Easier to develop large systems with multiple teams
- Smaller deployment artifacts make continuous delivery easier
- Easier replacement of large legacy systems

The **challenges of microservices are mostly to do with operation.** Strengths should be maximized and weaknesses should be minimized.

**Integration and communication between microservices are more complex than monoliths.** There is added technological complexity here.

## Micro and Macro Architecture

The **micro architecture** comprises all decisions that can be made individually for each microservice.

The **macro architecture** consists of all decisions that can be made at a global level and apply to all microservices.

### Domain Driven Design and Bounded Contexts

#### Domain Driven Design

DDD offers a collection of patterns for the domain model of a system.

#### Bounded Context

A **bounded context** is a submodule that has its own domain model.

#### Domain events between Bounded Contexts

Bounded contexts divide a system by domains.

### Strategic Design and Common Patterns

1. Bounded Contexts are contexts where the domain model is valid.
2. Bounded Contexts depend on each other in different ways.
3. The upstream team can influence the downstream team

### Architecture Decisions

#### Programming Languages, frameworks, and infrastructure

Can be defined by each microservice at each micro architecture.

Can also be defined at the macro architecture level if we want to.

#### Database

- Micro: Each microservice can also have its own instance of the database. If they are defined at the microarchitecture:
  - The crash of one database would cause only one microservice to crash.
  - Higher effort is a counter argument
- Macro: To avoid needing many different databases, the database can be defined by the macro architecture for all microservices
  - Even if the database is defined at the macro architecture level: **multiple microservices must not share a database schema**. This would violate the idea of bounded contexts.
  - Each microservice would need to have separate schemata in the database

#### User Interface

- Micro: sometimes a system has different types of users with different requirements. This can be addressed using the isolation of microservices.
- Macro: Often a system needs to have a uniform UI.
  - A **style guide** should be part of the macro architecture instead of trying to share HTML and JS

### Typical Macro Architecture Decisions

#### Communication Protocol

This is typically a macro architecture decision:

- Only if all microservices provide a  **uniform interface** can they communicate with each other effectively. Examples include REST and messaging.
- The **data formate must be standardized**

If the communication protocol was a microservice decision we would lose all sense of coherance.

#### Authentication

The entire infrastructure should use a **single authentication service**.

#### Integration

Integration testing is a macro level decision because we need to decide how to integrate all of the services under one roof.

### Typical Micro Architecture Decisions

#### Authorization

**Authorization determines what a user can do in a system**. Authorizations should be done **in their respective microservices**. Typically authorization is seen as a domain decision.

Authentication assigns the user roles used in authorization.

#### Testing

Testing can be different for each microservice. Even tests are ultimately part of the domain logic.

Since tests are different, the continuous delivery pipeline is different for each microservice.

##Operation: Micro or Macro Architecture?

### Configuration

We must define the interface with which a microservice obtains its configuration parameters.

These parameters include:

1. Technical parameters such as thread pool sizes
2. Parameters for the domain logic

The decision of how to store and generate these configuration parameters are independent of the data. They can either be generated for pulled from a database.

The information on which computer and under which port a service can be reached does not belong to the configuration but rather to the **service discovery**.

### Monitoring

Monitoring is the **toolset that managest metrics.** These metrics provide information about the state of the system. 

### Log Analyis

Defines a tool to manage logs. Logs are **now stored in specialized servers**. This makes them easier to analyze and search.

### Deployment

Determines **how the microservice is to be rolled out**. 

## Give preference to Micro Architecture

Try to make as many decisions as possible at the micro level.

### Macro Architecture decisions best practices

The fewer macro rules the higher the likelihood of success.

- For example it may be okay to define the monitoring technology but not how the metrics are measured in the application.
- Rules should be minimal
- Macro architecture rules must be enforced
- Keep independence of services a primary goal
- Complying with macro architecture is in the teams best interest

## Independent Systems Architecture Principles

### Principles for Microservice Architecture

1. The system must be divided into modules - Modules should define interfaces and should only be accessed through them. This allows each system to keep independent features like a data model.
2. Two separate levels of architectural decisions
   1. Macro Architecture - Decisions than concern all modules in the system
   2. Micro Architecture - Decisions that are made local to the services
3. Modules must be separate processes/containers/VMs - In a deployment monolith most of these decisions will be made on the macro level.
4. Standardized Integration and Communication - Integration and Communication options must be made on the macro level.
5. Standardized Metadata - Methods such as authentication must be standardized. Might be implemented as a token or dependent calls. Metadata must be able to be transferred between microservices and be a part of the macro architecture.
6. Independent Continuous Delivery Piplelines - Each module must have its own independent pipeline.
7. Operations Should be standardized -
   1. Configuration
   2. Deployment
   3. Log Analysis
   4. Tracing
   5. Monitoring
   6. Alarms
8. Standardized Interface - Standards for operations, integration and communication should be enforced on an interface level. For example: JSON format but every module should be free to use a different REST library or implementation.
9. Modules have to be resilient
   1. May not fail when other modules are unavailable or when communication problems occur.
   2. Must be able to shut down without losing data or state
   3. Must be possible to move them to other environments without breaking

# Docker

Docker and microservices are basically synonymous.

## Reasons why Docker and Microservices go together

Each microservices can run on its own virtial machine to achieve true isolation. Through virtualization each microservice has its own system installation. Thus the choice of network parameters and OS configuration can be isolated to each microservice.

### Overhead

A problem with this is that a VM has substantial overhead. 

### Ideal solution

A lightweight alternative to virtualization.

## Docker Basics

Docker is a lightweight alternative to virtualization. It is much more light weight than a VM.

Instead of having a VM of its own, Docker containers **share the kernel** of the OS where Docker is running. They appear as processes on the host machine.

### Isolated netowork of Dockers

Docker containers have their own network interface. The **same port** can be used in each Docker container. This is possible through a subnet on the host. You can **map ports** of individual Docker containers onto the host ports.

### Optimized File System

There are **layers in the file system** that are organized based on shallowness of need.

`Written Files -> Java app -> JDK -> Linux`

The microservices can only write to the shallowest level.

### One process per container

Docker instances are highly isolated. Therefore, **only one process should run in a Docker container**.

### Docker Image and Docker Registry

The file systems of Docker containers can be exported as Docker Images. These images can be passed on as files or stored in a Docker registry.

Alpine Linux is an ultra small Linux distro that is ideal for Docker instances.

## Docker Files

Docker reference stuff is available on the Internet.

An example of a Docker File:

```
FROM openjdk:11.0.2-jre-slim
COPY target/customer.jar .
CMD /usr/bin/java -Xmx400m -Xms400m -jar customer.jar
EXPOSE 8080
```

The FROM command loads images from the Docker hub on the Internet.

Docker can also be used as an **immutable server**.

An **idempotent install** is one where the installation script provides the same result no matter how often or when it runs.

## Docker Compose

A typical microservice system contains more than a single Docker container. One can use Docker Compose to start and run multiple containers together.

### Service Discovery with Docker Compose Links

Coordinating a system of Docker components **requires configurations for the virtual network** with which Docker containers use to communicate with each other. Containers must be able to find each other in order to communicate.

In a Docker Compose environment, a service can simply contact another service via a Docker Compose Link and then **use the service name as the host name.** So it could use a link like this to the Order microservice:

`http://order/`

Docker compose links also provide some kind of **service discovery**, a way for a microservice to find other microservices. Docker Compose Links also offer **Load Balancing**. It can also bind ports. It can also provide **volumes,** which are **file systems shared by multiple containers**.

### YAML configuration

Docker compose uses the `docker-compose.yml` file to configure itself:

```
version: '3'
services:
  common:
    build: ../scs-demo-esi-common/
  order:
    build: ../scs-demo-esi-order
  varnish:
    build: varnish
    links:
     - common
     - order
    ports:
     - "8080:8080"
```

# FIN

 