---
title: "Java - Spring Boot on Kubernetes"
description: "Java Spring Boot Web App containerize and deploy to Kubernetes"
date: 2021-02-23T22:08:25+08:00
draft: false
toc: true
images:
author: "Justin Hung"
type: ["posts", "post"]
tags:
  - Docker
  - Kubernetes
  - Java
  - Spring Boot
categories:
  - POC
  - Learning
series:
  - Kubernetes
---

## Prerequisites
Everyone will need:

Basic knowledge of Java, Gradle, Docker and Kubernetes
Components' version : 

* JDK 8 or higher
* Please ensure you have a JDK installed and not just a JRE
* Docker installed
* Kubectl installed
* IDE (IntelliJ, Eclipse, VSCode)

---
## Dockerize
| Makes original app build and run via Docker |
| --- |

### Spring Initializr
- Details : Spring initializr ()
- Clone project and add to IDE


    ``` shell
    curl https://start.spring.io/starter.tgz \
        -d language=java \
        -d javaVersion=1.8 \
        -d type=gradle-project \
        -d baseDir=springk8s \
        -d bootVersion=2.4.2 \
        -d packageName=com.example.springk8s\
        -d groupId=com.example \
        -d artifactId=springk8s\
        -d packaging=jar \
        -d dependencies=web,actuator,devtools,lombok,configuration-processor \
    | tar -xzvf -
    ```
- delete application.properties and add application.yaml


    ``` yaml
    server:
      port: 8080
      shutdown: "graceful"
    spring:
      application:
        name: ${info.app.name}
      lifecycle:
        timeout-per-shutdown-phase: "20s"
    management:
      endpoint:
        health:
          show-details: always
          probes:
            enabled: true
      metrics:
        tags:
          application: ${info.app.name}
    info:
      app:
        name: SpringK8s
        description: Spring Boot actuator for k8s cloud stack demo
        version: 1.0.0
        encoding: UTF-8
        java.version: 1.8
    ```
- gradle build projects
- run and test
- `curl -ivL http://localhost:8080/actuator/health`
- Result : 


    ``` json
    {
       "status":"UP",
       "components":{
          "diskSpace":{
             "status":"UP",
             "details":{
                "total":498851688448,
                "free":244886433792,
                "threshold":10485760,
                "exists":true
             }
          },
          "livenessState":{
             "status":"UP"
          },
          "ping":{
             "status":"UP"
          },
          "readinessState":{
             "status":"UP"
          }
       },
       "groups":[
          "liveness",
          "readiness"
       ]
    }
    ```
### Containerize Spring Boot App
The first step in running the app on Kubernetes is to write a Dockerfile and producing a container for the app we can then deploy to Kubernetes

1. Dockerfile
    - add Dockerfile to root project


      ```dockerfile
      FROM openjdk:8-jre-alpine

      WORKDIR /usr/app/

      COPY build/libs/*.jar app.jar

      EXPOSE 8080

      ENTRYPOINT ["java","-jar","app.jar"]
      ```
2. Build image:
    `docker build -t image/springk8s .`

3. Running image:
    `docker run -d -p 8080:8080 image/springk8s`

4. Test container:
    `curl http://localhost:8080/actuator/info`

5. Putting The Container In A Registry:
    `docker push / docker pull`

## Kubernetes
|Makes Docker image deploy to Kubernetes|
|---|

### Deploying To Kubernetes

Kubernetes uses YAML files to provide a way of describing how the app will be deployed to the platform

* You can write these by hand using the Kubernetes documentation as a reference
* Or you can have Kubernetes generate it for you using kubectl
* The --dry-run flag allows us to generate the YAML without actually deploying anything to Kubernetes
* Generate by command line : 
    ``` bash
    mkdir k8s
    kubectl create deployment k8s-demo-app --image localhost:5000/apps/demo -o yaml --dry-run=client > k8s/deployment.yaml
    ```
* Generate by yaml example as shown below

### K8s resources config best practice
- [Prerequisites](#prerequisites)
- [Dockerize](#dockerize)
  - [Spring Initializr](#spring-initializr)
  - [Containerize Spring Boot App](#containerize-spring-boot-app)
- [Kubernetes](#kubernetes)
  - [Deploying To Kubernetes](#deploying-to-kubernetes)
  - [K8s resources config best practice](#k8s-resources-config-best-practice)
    - [Externalized Configuration](#externalized-configuration)
    - [Lifecycle](#lifecycle)
    - [Liveness and Readiness Probes](#liveness-and-readiness-probes)
    - [Graceful Shutdown](#graceful-shutdown)
    - [Handling In Flight Requests](#handling-in-flight-requests)
    - [Resource Manage](#resource-manage)
    - [Service Discovery](#service-discovery)
    - [Configuration-watcher (bp)](#configuration-watcher-bp)
    - [HA : LoadBalancer](#ha--loadbalancer)
- [Reference](#reference)

#### Externalized Configuration
* Transfer application.yaml and active profiles to configmap
* Create ConfigMap using existing files
    - `DEPLOYMENT_ENV: dev`
    - `kubectl create configmap springk8s-app-config --from-file=application.yaml`
* Mount to container


    ``` yaml
    spec:
      containers:
        - name: springk8s
          image: image/springk8s
          imagePullPolicy: IfNotPresent
          args:
            - '--spring.profiles.active=$(DEPLOYMENT_ENV_KEY)'
            - '--spring.config.location=/usr/config/'
          ports:
            - containerPort: 8080
          volumeMounts:
            - name: springk8s-app-config
              mountPath: /usr/config
              readOnly: true
      volumes:
        - name: springk8s-app-config
          configMap:
            name: springk8s-profile-cm
            items:
              - key: application.yaml
                path: application.yaml
    ```

#### Lifecycle
- Liveness and Readiness Probes
- Graceful Shutdown
- Handling In Flight Requests

#### Liveness and Readiness Probes
* [spring actuator: Liveness + Readness](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-kubernetes-probes)

* application.yaml


    ``` yaml
    management:
      endpoints:
        web:
          exposure:
            include: "*"
      endpoint:
        health:
          show-details: always
          probes:
            enabled: true
      metrics:
        tags:
          application: ${info.app.name}
    ```

* deployment.yaml


    ``` yaml
    livenessProbe:
    httpGet:
      path: /actuator/health/liveness
      port: 8080
    initialDelaySeconds: 8
    readinessProbe:
    httpGet:
      path: /actuator/health/readiness
      port: 8080
    initialDelaySeconds: 8
    ```

#### Graceful Shutdown

* deployment.yaml


    ``` yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      ...
      name: k8s-demo-app
    spec:
    ...
      template:
        ...
        spec:
          containers:
            ...
            lifecycle:
              preStop:
                exec:
                  command: ["sh", "-c", "sleep 10"]
    ```

#### Handling In Flight Requests

* application.yaml


    ``` yaml
    server:
      port: 8085
      shutdown: "graceful"
    ```


#### Resource Manage
* [JVM in a Container](https://merikan.com/2019/04/jvm-in-a-container/)
* Manage resource in k8s


    ``` yaml
    spec:
          imagePullSecrets:
            - name: regcred
          containers:
            - name: springk8s
              image: image/springk8s
              imagePullPolicy: IfNotPresent
              args:
                - '--spring.profiles.active=$(DEPLOYMENT_ENV_KEY)'
                - '--spring.config.location=/usr/config/'
              ports:
                - containerPort: 8080
              env:
                - name: DEPLOYMENT_ENV_KEY
                  valueFrom:
                    configMapKeyRef:
                      name: springk8s-profile-cm
                      key: DEPLOYMENT_ENV
              resources:
                requests:
                  memory: "256Mi"
                  cpu: "300m"
                limits:
                  memory: "512Mi"
                  cpu: "500m"
    ```
* Resources : memory limits + Java heap size?
<!-- TODO -->

#### Service Discovery
* Kubernetes makes it easy to make requests to other services
* Each service has a DNS entry in the container of the other services allowing you to make requests to that service using the service name
* For example, if there is a service called k8s-workshop-name-service deployed we could make a request from the k8s-demo-app just by making an HTTP request to http://k8s-workshop-name-service

* Testing 
* `kubectl port-forward service/k8s-demo-app 8080:80`
* `curl http://localhost:8080`
* Istio : <!-- TODO -->

#### Configuration-watcher (bp)

<!-- TODO -->

#### HA : LoadBalancer

<!-- TODO -->

`watch -n 1 kubectl get all`

## Reference
- [Spring on Kubernetes!](https://hackmd.io/@ryanjbaxter/spring-on-k8s-workshop)
- [Spring Boot Actuator: Production-ready Features](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-kubernetes-probes)
- [JVM in a Container](https://merikan.com/2019/04/jvm-in-a-container/)
- [Java 安裝在 Docker 容器的記憶體問題](https://nullpointerexception.tangblack.com/java-%E5%AE%89%E8%A3%9D%E5%9C%A8-docker-%E5%AE%B9%E5%99%A8%E7%9A%84%E8%A8%98%E6%86%B6%E9%AB%94%E5%95%8F%E9%A1%8C/)