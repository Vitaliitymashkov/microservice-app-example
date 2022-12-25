# Example microservice app

https://github.com/elgris/microservice-app-example.git

This is an example of web application comprising of several components communicating to each other. In other words, this is an example of microservice app. Why is it better than many other examples? Well, because these microservices are written in different languages. This approach gives you flexibility for running experiments in polyglot environment.

The app itself is a simple TODO app that additionally authenticates users. I planned to add some admin functionality, but decided to cut the scope and add it later if needed.

## Components

1. [Frontend](/frontend) part is a Javascript application, provides UI. Created with [VueJS](http://vuejs.org)
2. [Auth API](/auth-api) is written in Go and provides authorization functionality. Generates JWT tokens to be used with other APIs.
3. [TODOs API](/todos-api) is written with NodeJS, provides CRUD functionality ove user's todo records. Also, it logs "create" and "delete" operations to Redis queue, so they can be later processed by [Log Message Processor](/log-message-processor).
4. [Users API](/users-api) is a Spring Boot project written in Java. Provides user profiles. Does not provide full CRUD for simplicity, just getting a single user and all users.
5. [Log Message Processor](/log-message-processor) is a very short queue processor written in Python. It's sole purpose is to read messages from Redis queue and print them to stdout
6. [Zipkin](https://zipkin.io). Optional 3rd party system that aggregates traces produced by other components.

Take a look at the components diagram that describes them and their interactions.
![microservice-app-example](https://user-images.githubusercontent.com/1905821/34918427-a931d84e-f952-11e7-85a0-ace34a2e8edb.png)

## Use cases

- Evaluate various instruments (monitoring, tracing, you name it): how easy they integrate, do they have any bugs with different languages, etc.

## How to start

The easiest way is to use `docker-compose`:

```
docker-compose up --build
```
    
    NOTE
    If project doesn't compile because of an error "/bin/sh: ./mvnw: not found", please check/update/change IDEA settings for CRLF to LF

![IDEA - LF setting - In case of *./mvnw not found issue*](/assets/img/CRLFvsLF.png "CRLFvsLF")

Credit: "https://github.com/docker/docs/issues/13930#issuecomment-1174904623"

For any n00bs like me who just spent ages looking through the mvnw file trying to figure out where these \r\n were, this is a Windows/Unix line endings thing so 'r' as in 'return' and 'n' as in new line, aka Carriage Return and Line Feed, CR LF - Windows uses CRLF for line endings wheras Unix/Linux only uses LF. In VS Code the fix is as simple as opening the file and pressing a little toggle in the bottom right corner, then saving the file. I'm probably the only person dumb enough not to realise that, but I figured I'd put this here just in case.
    



Then go to http://127.0.0.1:8080 for web UI. [Zipkin](https://zipkin.io) is available on http://127.0.0.1:9411 by default.

## Contribution

This is definitely a contrived project, so it can be extended in any way you want. If you have a crazy idea (like RESTful API in Haskell that counts kittens of particular user) - just submit a PR.

## License

MIT



# Internal info
## DevSlop Kubernetes CTF WriteUp

https://cybersecfaith.com/2021/02/21/devslop-kubernetes-ctf-writeup/

Table of Contents
1. Getting Started
2. Building Docker Images
3. Pushing Docker Images to Amazon ECR Repositories
4. Configuring Environment Variables with ConfigMaps and Secrets
5. Deploying Services
6. Deploying Microservices to Kubernetes
7. Deploying Redis
8. Configuring Ingress For The Front-End
9. Securing the Cluster with Network Policies

### Getting Started
The DevSlop Game Day Announcement advised us to install the following tools prior to Game Day:

The AWS CLI version 2
The Kubernetes command-line tool
Docker
An IDE or Text Editor of choice eg.Visual Studio Code, Sublime Text, Atom etc.
