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

<br/>
</br>

 <sub/> Next we can to open a new browser tab and proexy a request on port 8080 using the built-in Web Preview feature in Cloud Shell. After that a new browser tab will open to display your results. Before continuing we should stop stop the running node server by typing CTRL+C in Cloud Shell. </sub>

