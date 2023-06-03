## Install Kind and creat Cluster

# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.19.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.19.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

kind create cluster --image kindest/node:v1.22.12

docker ps

docker exec -it <kind-container> bash

kubectl get nodes

## Create Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-pod
spec:
  containers:
  - name: app-container
    image: nginx:latest
    ports:
    - containerPort: 80
```
  
kubectl get pod -A -o wide

## Create ReplicaSet
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: app-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: app-container
        image: nginx:latest
        ports:
        - containerPort: 80
```
  
kubectl edit replicaset 

## Create Deployment
  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: app-container
        image: nginx:1.19.10
        ports:
        - containerPort: 80
```

## DeamonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: app-daemonset
spec:
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: app-container
        image: nginx:latest
        ports:
        - containerPort: 80
```
  
## Create Service
  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: app-container
        image: nginx:1.19.10
        ports:
        - containerPort: 80
```
## Configmap
  
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-configmap
data:
  index.html: |
    <html>
    <body>
    <h1>Welcome to my custom Nginx page!</h1>
    </body>
    </html>
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.19.10
        ports:
        - containerPort: 80
        volumeMounts:
        - name: nginx-config-volume
          mountPath: /usr/share/nginx/html
          readOnly: true
      volumes:
      - name: nginx-config-volume
        configMap:
          name: nginx-configmap
```
  
## Assigment

Certainly! Here's an assignment you can provide after completing the hands-on labs on Kubernetes:

Assignment: Deploy a Multi-tier Application on Kubernetes

Objective:
The objective of this assignment is to apply the concepts learned in the hands-on labs and deploy a multi-tier application on Kubernetes. The application consists of a frontend web server, a backend API server, and a database.

Instructions:
1. Design and create the necessary YAML files to deploy the following components:
   - Frontend Deployment and Service: Deploy a frontend web server (e.g., Nginx) with multiple replicas. Expose the frontend service to access it from outside the cluster.
   - Backend Deployment and Service: Deploy a backend API server (e.g., Node.js, Flask, or any other framework) with multiple replicas. Expose the backend service within the cluster.
   - Database Deployment and Service: Deploy a database (e.g., MySQL or PostgreSQL) with persistent storage for data persistence. Expose the database service within the cluster.

2. Configure the appropriate networking between the frontend, backend, and database components.
   - The frontend should be able to communicate with the backend API server.
   - The backend API server should be able to access the database for data storage and retrieval.

3. Test the application by accessing the frontend web server from outside the cluster and verifying that it communicates with the backend API server and retrieves data from the database.

4. Document the steps followed to deploy the application, including the YAML files created and any necessary configuration details.

5. Submit the documentation and the YAML files as the assignment deliverables.

Additional Tips:
- Use appropriate labels and selectors to ensure proper service discovery and communication between components.
- Pay attention to resource requirements and limits to ensure the application runs smoothly within the cluster.
- Consider using Secrets to store sensitive information like database credentials.
- Test the application thoroughly to ensure all components are functioning as expected.

Note:
You can choose any suitable technologies and frameworks for the frontend, backend, and database components. Feel free to get creative and add additional features or enhancements to the application if you wish.

Good luck with the assignment, and if you have any questions, feel free to ask for assistance!
