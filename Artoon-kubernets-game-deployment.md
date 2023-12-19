# **Artoon Kubernetes Deployment**

## Artoon Games project Deployment Using Kubernetes

### Clone The Projcet In kubernetes Server

```bash
# cd /home/node/
# git clone <git url>
```

### Node Project Need .env file or need config.json file to add credential

### Create All Credential In Server And Add In config.json file or add .env file Depends On Project

```text
1. mongodb  : DataBase, User, Password,
2. redis    : DB=NO. 
3. rebbitMQ : User, Password, VirtualHost
```

## Copy This file from diamond-connect-api project

```text
1. Dockerfile
2. node-rolling-update.yml
3. service.yml
```

- ### change your data in this 3 file with this project requirment

## **1. Dockerfile :-**
  
### All Starts With A Base Image to run an app. A base image and all its dependencies are described in a file called "Dockerfile." the Dockerfile looks like this. Take a look :-

```docker
FROM node:10.20.0
RUN mkdir -p /usr/scr/app 
RUN chown -R node:node /usr/scr/app
WORKDIR /usr/scr/app
COPY package*.json ./
COPY . .
RUN chown -R node:node .
RUN npm install
USER node
EXPOSE 6155
CMD [ "node", "app.js" ]
```

## Details about Dockerfile :-

### We Need To Change Following Things From Docker File

- ### **From** : It's Sets the base image  for subsequent instructions. a valid Dockerfile must start with a FROM instruction. The image can be any valid image – it is especially easy to start by pulling an image from the Public Repositories

- ### **RUN** : The RUN instruction will execute any commands in a new layer on top of the current image and commit the results

- ### **WORKDIR** : The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile

- ### **EXPOSE** : The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime

- ### **npm install** : Make Sure install node modules in the end

### **Now We Need To build Image**

```text
# npm install
# npm start
# npm run build
```

## **2. node-rolling-update.yml :-**

### *Pods represent the processes running on a cluster Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.*

### **Create node-rolling-update.yml file As bellow mention.**

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: name
  labels:
    app: name
spec:
  replicas: 1
  selector:
    matchLabels:
      app: name
  template:
    metadata:
      labels:
        app: name
    spec:
      containers:
        - name: name
          image: images:tag
          ports:
            - containerPort: port_number
      imagePullSecrets:
        - name: mahipal
```

- ### **need to define basic names as project name change it**

## **reference**

### We Need To Change Following Things From node-rolling-update.yml file

- ### **image** : Provide image name here which Pushed On Container registory

- ### **containerPort** : Define Port which container listens on

- ### **imagePullsecrets** : Secret Help to pull image from gitlab container registory

### **Creating and checking status Pod With Bellow Mentioned Commands**

## **SERVICE :-**

- ### Service is An abstract way to expose an application running on a set of Pods as a network service

### **Create service.yml file As bellow mention.**

### **service.yml :-**

```yml
apiVersion: v1
kind: Service
metadata:
  name: name
spec:
  type: NodePort
  selector:
    app: name # selector for deployment
  ports:
  - name: example-port
    protocol: TCP
    port: 1234 # CLUSTERIP PORT
    targetPort: define_containerPort # POD PORT WHICH APPLICATION IS RUNNING ON
    nodePort : define_node_port #here!
```

## **Need To Add All Ports**

- ### **port** : Cluster Port Same For All Deployments

- ### **nodeport** : A node port exposes the service on a static port on the node IP address. NodePorts are in the 30000-32767 range by default, which means a NodePort is unlikely to match a service’s intended port

```bash
# kubectl apply -f service.yml
# kubectl apply -f node-rolling-update.yml 
```

```bash
# kubectl get svc  - For check free port 
# kubectl get svc | grep --port-- = check this port are use in which project
```

### Go to the gitlab repository and click on **Settings** > **CI/CD** and click on **Variables** and add

### Go to the gitlab repository and click on **Packeges & Registory**> **Container Registory** and copy commands and execute one by one

### Example :-

```bash
# docker login gitlab.artoon.in:6000
# docker build -t gitlab.artoon.in:6000/
# docker push gitlab.artoon.in:6000/
```

### **Check Your Pods And Service Are Running And Check Your Deployment Through Node Port**

## **CI/CD**

```yml
stages:
  - deploy
  - build-image

deploy_dev:
  stage: deploy
  allow_failure: false
  tags:
    - kube-ssh
  environment:
    name: dev
    url: http://kube.artoon.in:node_port/ - port
  script:
    - cd /home/node/project_dir/
    - git pull http://$usr:$pwd@gitlab.artoon.in/node-js/.../ - Project Git URL
  when: manual

image: docker:stable
docker_build:
  stage: build-image
  tags:
    - kube-ssh
  script:
    - cd /home/node/project_dir
    - docker build -f Dockerfile -t $CI_REGISTRY_IMAGE:latest .
    - echo $CI_REGISTRY_IMAGE
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $CI_REGISTRY_IMAGE:latest
    - echo $DOCKER_IMG
    - kubectl rollout restart deploy/deployment_name(pod)
  after_script:
    - curl --request DELETE --data 'name_regex_delete=.*' --data "keep_n=4" --header "$PRIVATE $TOKEN" "http://gitlab.artoon.in/api/v4/projects/3563/registry/repositories/41/tags"
  when: manual
```
