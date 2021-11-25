# A Beginner's Guide To Kubernetes

Kubernetes is an open source system for deploying, scaling and managing multi-container applications. This sample code is an elaboration of multiple components such as the backend,
frontend and the database services can be managed in a K8S cluster.

`kubernetes` folder would contain the declarative approach for the deployments and services configured.

#### Deployments

Deployments are configurations for the desired state we need to achieve when deploying the container.
Here's an example of a deployment configuration YAML file

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
spec:
  replicas: 1
  selector: 
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend
          image: academind/kub-demo-frontend:latest
```

`spec` stands for specifications, it contains the container image for the frontend application which can be pulled from the docker hub registry (as in `academind/kub-demo-frontend:latest`)

#### Services

Services are a means to access the applications in the cluster. We can either access them using the cluster internal IP, node port or load balancer. Cluster IP may change when during
re-deployment, whereas node ports may remain the same unless and until we dont use multiple nodes in the cluster. Therefore, the best way to go is with a load balancer which
would distribute the load among different replicas and make sure our application is accessible irrespective of the IP address or node

Here's an example of a service to access the frontend application

```

apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

The frontend application is run in port 80 of the container, therefore it's exposed to the port 80 of our network.
