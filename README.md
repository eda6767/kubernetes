# Kubernetes

<sub/>
Kubernetes Engine, often referred to as GKE (Google Kubernetes Engine), is a managed Kubernetes service provided by Google Cloud Platform (GCP). It simplifies the deployment, management, and scaling of containerized applications using Kubernetes. </sub>

<sub/> In following scenario we are going to  turn code  into a replicated application running on Kubernetes, which is running on Kubernetes Engine. For this repository the code will be a simple Hello World node.js app. </sub>


<p align="center">
<img width="600" alt="Zrzut ekranu 2024-01-14 o 11 07 16" src="https://github.com/eda6767/kubernetes/assets/102791467/349bcc1b-8603-4d69-8353-28e117044c44">
</p>


<br/>
</br>

<sub/> In the first step we need to create in Cloud Shell a simple Node.js server, which later will be deployed to Kubernetes Engine </sub>

<sub/> 

```
vi server.js
```

</sub>

<br/>
</br>

<sub/>

```
var http = require('http');
var handleRequest = function(request, response) {
  response.writeHead(200);
  response.end("Hello World!");
}
var www = http.createServer(handleRequest);
www.listen(8080);
```

</sub>


<br/>
</br>


 <sub/> Because Cloud Shell has the node executable installed, we can immediatelly start the node server </sub>

<sub/>

```
node server.js
```
</sub>


 <sub/> Next we can to open a new browser tab and proexy a request on port 8080 using the built-in Web Preview feature in Cloud Shell. After that a new browser tab will open to display your results. Before continuing we should stop stop the running node server by typing CTRL+C in Cloud Shell. </sub>



 <sub/> Next step is to create a Dockerfile that describes the image you want to build. Docker container images can extend from other existing images, so for this image, we'll extend from an existing Node image: </sub>

<sub/> 

 ```
vi Dockerfile
```

</sub>

<sub/> 
After created file, we can add content - start from the node image found on the Docker, expose port 8080, copy file server.js to the image, and last command : start the node server. 


```
FROM node:6.9.2
EXPOSE 8080
COPY server.js .
CMD node server.js
```


</sub>

<sub/> 
Next step is to build the image using following command - replacing PROJECT_ID with you project id. And then run a Docker container as a daemon on port 8080 from your newly-created container image. </sub>



<sub/> 

```
docker build -t gcr.io/PROJECT_ID/hello-node:v1 .
docker run -d -p 8080:8080 gcr.io/PROJECT_ID/hello-node:v1
```
 </sub>


<sub/> To check the result - use web preview feature or run the following command: </sub>
 
<sub/>

```
curl http://localhost:8080
```
</sub>

<sub/> Next step is to stop the running container. To to this, we need to find our Docker container ID, and then stop it by given containder id value. </sub>

<sub/>

```
docker ps
docker stop [CONTAINER ID]
```
</sub>

<sub/>  Now when the image is working as intended, we can push it to the Google Container Registry, a private repository for your Docker images. </sub>

<p align="center">
<img width="826" alt="Zrzut ekranu 2024-01-14 o 16 38 07" src="https://github.com/eda6767/kubernetes/assets/102791467/9943312d-5c50-410a-b965-f8c6a7068afc">


</p>


<sub/>  

```
gcloud auth configure-docker
docker push gcr.io/PROJECT_ID/hello-node:v1
```
</sub>


<sub/>  Since now the container image will be listed in Container Registry. Therefore we have a project-wide Docker image available which Kubernetes can access and orchestrate. Next step is to create Kubernetes Engine cluster. A cluster consists of a Kubernetes master API server hosted by Google and a set of worker nodes. The worker nodes are Compute Engine virtual machines. </sub>

<sub/> 

```
gcloud config set project PROJECT_ID

gcloud container clusters create hello-world \
                --num-nodes 2 \
                --machine-type n1-standard-1 \
                --zone "ZONE"
```
</sub>



<sub/> Since we created cluster it's time to deploy a containerized application to the Kubernetes cluster! From now on we are gonna use the kubectl command line. A Kubernetes pod is a group of containers tied together for administration and networking purposes. It can contain single or multiple containers. Here we will use one container built with the Node.js image stored in the private container registry. It will serve content on port 8080. </sub>

<sub/>

```
kubectl create deployment hello-node \
    --image=gcr.io/PROJECT_ID/hello-node:v1
```

</sub>

<sub/>

```
kubectl get pods
kubectl cluster-info
kubectl config view
kubectl get events
kubectl logs <pod-name>
```

</sub>
