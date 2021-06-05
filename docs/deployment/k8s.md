<h1 align="center">
<img src="https://www.vhv.rs/dpng/d/484-4842971_kubernetes-logo-png-kubernetes-logo-vector-transparent-png.png" alt="drawing" width="200">
<br><br>Kubernetes orchestration
</h1>

## Description

<p>Docker is a good solution for running applications in cloud,
but it does not solve such problems as Service Discovery, Load Balancing and Failover. 
But kubernetes does, therefore, I need to pack docker containers 
inside k8s pods which will be managed by k8s. 
The advantages of such attitude is that all the responsibilities is on k8s, 
we don't need to think how to scale instances or what to do if one of them is not healthy.</p>

<!-- https://shields.io/ -->

## Structure

First, I need a Kubernetes cluster I will deploy my services to, I use Google Kubernetes Engine (GKE) for that.
In order to deploy my pods, I need to create service_name-deployment.yaml files for every service.
Here's an example of logiweb-deployment.yaml:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: logiweb-service
  labels:
    app: logiweb-service
spec:
  replicas: 1
  selector:
    matchLabels:
      app: logiweb-service
  template:
    metadata:
      labels:
        app: logiweb-service
    spec:
      containers:
        - name: logiweb-service
          image: tuanalexeu/logiweb-service:final-task3
          ports:
            - containerPort: 8080

```

We can specify number of replicas, that is, if one instance fails, the load balancer will forward all request to healthy ones. 
Also, we need to specify docker image we want to deploy as pod.<br>
After running `kubectl apply -f service_name-deployment.yaml` we are finally able to expose the service to the internet.