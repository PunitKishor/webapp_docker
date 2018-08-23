# dotnet core + docker + k8s

Sample project to create dotnet core 2.0 mvc webapp docker image

## prerequisites

docker needs to be install to use this repo.

## dotnet core + docker

If you have dotnet core 2.0 sdk install locally on your machine the you can use following commands to create docker image.

Rename DockerFile.1 to DockerFile and execute following command in the root folder of the repo to create docker image.

```
dotnet restore
dotnet publish -c Release -o out
docker build . -t [imageName:version]
e.g.
docker build . -t webapp:v1
```

## docker

If you DO NOT have dotnet core 2.0 sdk install locally on your machine the you can use following commands to create docker image.
execute following command in the root folder of the repo to create docker image.

Rename DockerFile.2 to DockerFile and execute following command in the root folder of the repo to create docker image.

```
docker build . -t [imageName:version]
e.g.
docker build . -t webapp:v1
```

## run docker container

use folowing command to run the docker container.

```
docker-compose up -d
```

to kill the running docker run following commands.

```
docker-compose down
```

## access application

then web application can be accessed as 

```
http://localhost:5000
```

to check whether the environment are set properly in docker container

```
http://localhost:5000/Home/About/FIRST_NAME
http://localhost:5000/Home/About/LAST_NAME
```

## k8s

### prerequisites

to run a container in kubernetes, minikube and kubectl is required.


to run the created docker image in local minikube setup, execute steps to create image in minikube after this command
(or follow these stepsto share local image with minikube https://blog.hasura.io/sharing-a-local-registry-for-minikube-37c7240d0615 )


```
eval $(minikube docker-env)
```

Once image is ready in minikube
run this command to create deployment

without deployment.yml
```
kubectl run webapp --image=webapp:v1 --port 80
```

or using deployment.yml file
```
kubectl create -f deployment.yml
```

once deployment is ready run this command to verify the pod is running fine

```
kubectl get pods
```

after this we need to expose k8s deployment as a service so run this command

```
kubectl expose deployment webapp --type=NodePort --port 80 --target-port 80
```

to test the applications run this command

```
minikube service webapp
```

to edit the deployment run this command

```
kubectl edit deployment.app/webapp
```

and delete the pod

```
kubectl get pods
```

kubectl delete pod [podname]

## Cleanup

### docker

```
docker rm webapp
```
```
docker rmi webapp:v1
```

### k8s

```
kubectl delete service webapp
```

```
kubectl delete deployment webapp
```






