# 04.Orchestration

## Introduction to Orchestration

Managing a single container is straightforward. But as your containerized applications grow, complexity arises. This is where container orchestration comes in.

### What is Container Orchestration?

Imagine an orchestra conductor leading a complex musical piece. Similarly, container orchestration automates the deployment, scaling, networking, and management of containerized applications. It acts as a central maestro, ensuring all your containers run harmoniously within a distributed environment.

### Why is Orchestration Important?

- **Scalability:** Easily scale applications up or down by adding or removing containers as needed.
- **High Availability:** Ensures applications remain available even if individual containers fail by restarting them on healthy nodes.
- **Resource Management:** Optimizes resource utilization by efficiently placing containers on nodes based on their resource requirements.
- **Automated Workflows:** Automates deployments, rollbacks, and service updates, streamlining your container lifecycle management.

## Docker Swarm: Native Orchestration for Docker Users

Docker Swarm is a built-in clustering and orchestration tool within Docker itself. It provides a lightweight solution for deploying and managing services on a Swarm cluster. Let's delve into the key concepts:

- **Swarm Mode:** Enables a group of Docker engines to function as a single cluster, coordinating container management across all nodes.
- **Nodes:** Individual Docker engines participating in the Swarm cluster, classified as either manager nodes (responsible for cluster leadership) or worker nodes (responsible for running containers).
- **Services:** Long-running, scalable units of work built from containers. You can define how many replicas of a service should run for high availability.
- **Tasks:** Replicas of a service instance running on a specific node within the cluster.
- **Networks:** Logical networks for container communication within the Swarm cluster, enabling containers to interact with each other.

Docker Swarm offers a familiar environment for those already comfortable with Docker. Its lightweight nature makes it suitable for smaller-scale deployments or those seeking a quick and easy container orchestration setup.

## Hands-on Lab: Setting Up and Working with Docker Swarm

### Prerequisites:

- A Linux distribution with administrative privileges on multiple machines (or VMs).
- Basic understanding of Docker commands.

### Steps:

1.**Installation (on all machines):**

```bash
sudo apt-get update  # for Debian/Ubuntu
sudo yum update      # for Red Hat/CentOS
```
Follow official Docker installation guide for your Linux distribution: https://docs.docker.com/engine/install/

Install Docker Engine on all machines you intend to use in your Swarm cluster.

2.**Enable Docker Swarm Mode (on the manager node):**
```bash
docker swarm init
```
This initializes Docker Swarm mode on the current machine, making it the manager node. Copy the provided join command from the output for adding worker nodes.

3.**Join Worker Nodes:**

On other machines (designated as worker nodes), run the join command obtained from the manager node's output:

### Example join command (replace with actual output)
```bash
docker swarm join --token SWMTKN-1 ... 192.168.1.100:2377
```
4.**Deploy a Service:**
**1. Create a simple web application Dockerfile:**
```Dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
**2. Create an index.html file with some basic content.**

**3. Build the image:**
```bash
docker build -t my-web-app .
```
**4. Deploy a service with three replicas on the swarm:**
```bash
docker service create --replicas 3 --name my-web-app my-web-app:latest
```
## View Services and Tasks:**
 **List services:**
  ```bash
  docker service ls
```
 **Inspect a Service for Details:**

To inspect a service and retrieve detailed information about it, use the following command:
```bash
docker service inspect my-web-app
```
 **List Tasks (Running Instances of the Service):**

To list the tasks, which represent the running instances of a service, use the following command:
```bash
docker service ps my-web-app
```
**Scale the Service:**

To scale your web application service to five replicas, use the following command:
```bash
docker service scale my-web-app=5
```
## Docker Swarm: Essential Commands

### Cluster:

- `docker swarm init`: Create a manager node.
- `docker swarm join`: Join a worker node (with token).
- `dock.er node ls`: List all nodes (manager/worker).
- `docker node inspect <node_name>`: Get node details.
- `docker swarm leave`: Leave the Swarm cluster.
- `docker swarm unlock`: Unlock a locked cluster.

### Services:

- `docker service create <options> <image>:<tag>`: Create a service.
- `docker service ls`: List all services.
- `docker service inspect <service_name>`: Inspect a service.
- `docker service ps <service_name>`: View running service tasks.
- `docker service scale <service_name>=<replicas>`: Scale service replicas.
- `docker service update <service_name> <options>`: Update service config.
- `docker service rm <service_name>`: Remove a service.

### Networks:

- `docker network create <network_name> [--driver overlay]`: Create an overlay network (default driver in Swarm mode).
- `docker network ls`: List all networks.
- `docker network inspect <network_name>`: Inspect a network.
- `docker network rm <network_name>`: Remove a network.

### Logs & Debugging:

- `docker service logs <service_name>`: Get service logs.
- `docker node logs <node_name>`: Get node logs.

### Secrets & Configs:

- `docker secret create <secret_name> <file_or_content>`: Create a secret.
- `docker config create <config_name> <file_or_content>`: Create a config.
- `docker secret ls`: List all secrets.
- `docker config ls`: List all configs.

### Additional Tips:

- Use `--help` for detailed command usage.
- Refer to Docker docs for comprehensive explanations: [Docker Swarm CLI Reference](https://docs.docker.com/reference/cli/docker/swarm/)

## Introduction to Kubernetes: Orchestrating Your Containerized Applications

Kubernetes, often referred to as "K8s," has become the de facto standard for container orchestration. It automates the deployment, scaling, and management of containerized applications across a cluster of machines. This introduction dives into the core concepts of pods, deployments, ReplicaSets, services, and service types, along with essential commands for interacting with Kubernetes.

## 1. Understanding Pods: The Basic Units

Imagine a pod as a single container (like a studio apartment) or a group of co-located containers (like roommates sharing an apartment) that share storage and network resources. Pods are the smallest deployable unit in Kubernetes.

### Example YAML:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```
This YAML defines a pod named my-nginx-pod running a single Nginx container on port 80.
### Useful Commands for Pods

Here are some useful commands for interacting with pods in Kubernetes:

- `kubectl get pods`: Lists all pods in the current namespace.
- `kubectl describe pod <pod-name>`: Provides detailed information about a specific pod.
- `kubectl logs <pod-name>`: Retrieves the logs of a specific pod.
- `kubectl exec -it <pod-name> -- /bin/bash`: Opens an interactive shell session in a specific pod for troubleshooting or debugging.
- `kubectl delete pod <pod-name>`: Deletes a specific pod.
- `kubectl get pod <pod-name> -o yaml`: Retrieves the YAML definition of a specific pod.

These commands are essential for managing and troubleshooting pods in Kubernetes.

## 2. Services: Exposing Pods for External Access

In Kubernetes (K8s), services are an abstraction that enables network access to a set of pods in a consistent manner. They provide a stable endpoint for accessing the pods, even as the underlying pods may change due to scaling, rolling updates, or failures. Services play a crucial role in facilitating communication between different parts of an application, both within the cluster and with external clients.

## Types of Services in Kubernetes:
### 1. ClusterIP Service (Internal Communication)

Imagine a multi-tier application with a separate Pod for a MySQL database and a Node.js web application. The web application needs to connect to the database within the cluster.

### Example YAML:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-mysql-service
spec:
  selector:
    app: mysql # Selects Pods with label "app: mysql" (database)
  type: ClusterIP # Internal communication
  ports:
  - port: 3306 # Service port (can be different from database port)
    targetPort: 3306 # Target port on the MySQL Pods
```
### Explanation:

- This Service definition exposes the MySQL database Pods internally within the Kubernetes cluster using the name `my-mysql-service`.
- Other components, such as the web application Pods, can discover and communicate with the MySQL database through this Service.
- The Service acts as a stable endpoint for accessing the database, abstracting away the details of the individual Pods.
- The web application Pods can connect to the database using the Service name `my-mysql-service` and the specified port `3306`, which is the port the MySQL database is listening on inside the Pods.

  ## 2. NodePort Service (Simple External Access)

Imagine a basic web service Pod you want to make accessible from the internet.

### Example YAML:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-web-service
spec:
  selector:
    app: webserver # Selects Pods with label "app: webserver"
  type: NodePort # External access
  ports:
  - port: 80 # Service port (commonly used for web traffic)
    targetPort: 8080 # Target port on the webserver Pods (could be different)
    nodePort: 30080 # Specific NodePort (optional)
```
### Explanation:

- This Service definition exposes the web service Pod through a random NodePort (e.g., 30080) on each worker node in the cluster.
- To access the service from outside the cluster, users would use the following format: `<node_IP>:<NodePort>`, for example, `192.168.1.10:30080`.
- It's important to configure firewall rules on each node to allow traffic on the NodePort, ensuring that external traffic can reach the service.

## 3. LoadBalancer Service (Production Deployment)

Imagine a critical e-commerce application requiring high availability and scalability.

**Example YAML:** (Specific configuration may vary by cloud provider)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-ecommerce-app
spec:
  selector:
    app: ecommerce # Selects Pods with label "app: ecommerce"
  type: LoadBalancer # Production deployment
  ports:
  - port: 80 # Service port
    targetPort: 8080 # Target port on the Pods
```
### Explanation:

- This Service definition creates an external load balancer that distributes traffic across the Pods running the e-commerce application.
- The specific configuration for the load balancer, such as provisioning and setup, would be handled by your cloud provider, and the instructions might differ based on the provider's platform.
- External users would access the application using the DNS name provided by the load balancer, which routes traffic to the Pods behind the service.

## 4. ExternalName: (Redis server)
   Imagine you have a microservices application deployed on AKS that utilizes caching heavily. You're using Azure Cache for Redis as your caching solution, but it's hosted separately from your AKS cluster.
   
Note: This is a basic example and might require adjustments based on your specific application and requirements.

### 1. Python Flask Application (flask_app.py):
```python
from flask import Flask, request
import redis

# Configure with the Service name (not the actual Cache for Redis DNS name)
cache = redis.Redis(host="my-cache-redis", port=6379, db=0)

app = Flask(__name__)

@app.route('/')
def hello_world():
    cache.set("key", "value")
    cached_value = cache.get("key")
    return f'Data from Cache: {cached_value}'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)  # Listen on all interfaces
```
This Python Flask application demonstrates how to interact with a Redis cache using a Kubernetes Service name (my-cache-redis). You can include this code in your project and deploy it to Kubernetes. Make sure to replace "my-cache-redis" with the actual Service name of your Redis cache.

This Flask application retrieves data from the cache using the Service name (my-cache-redis) instead of the actual Azure Cache for Redis DNS name.

# 2. Create a Dockerfile:

```Dockerfile
FROM python:3.9

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "flask_app.py"]
```
### Explanation:

- This Dockerfile defines a Python 3.9 image as the base image for building the application container.
- It installs the necessary dependencies for the application, including Flask and Redis, using the `pip` package manager.
- The application code (`flask_app.py`) is copied into the container.
## 3. requirements.txt:
```bash
Flask
redis
```
# 4. Build the Docker Image:
```bash
docker build -t my-flask-app:latest .
```
# 5. YAML configuration (including ExternalName Service and Deployment):
```yaml
# ExternalName Service referencing Azure Cache for Redis
apiVersion: v1
kind: Service
metadata:
  name: my-cache-redis
spec:
  type: ExternalName
  externalName: "<your-cache-name>.redis.cache.azure.net"  # Replace with your actual cache name
```
 ## Deployment for your Flask application
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-flask-app
spec:
  replicas: 2  # Adjust the number of replicas as needed
  selector:
    matchLabels:
      app: my-flask-app
  template:
    metadata:
      labels:
        app: my-flask-app
    spec:
      containers:
      - name: my-flask-app
        image: my-flask-app:latest
        ports:
        - containerPort: 5000
```
### Explanation:

- The first section of the provided YAML file defines an ExternalName Service named `my-cache-redis`, which references your Azure Cache for Redis using its DNS name. You need to replace the placeholder with the actual DNS name of your Azure Cache for Redis instance.
- The second section defines a Deployment for your Flask application with two replicas. You can adjust the number of replicas as needed.

### Deployment Steps:

1. Replace placeholders in the YAML file with your specific details, such as the DNS name of your Azure Cache for Redis.
2. Build the Docker image for your Flask application using the `docker build` command.
3. Apply the YAML file to your AKS cluster using `kubectl apply -f your-deployment.yaml`.

# Useful Commands for Kubernetes Services

Here are some useful commands for interacting with Services in Kubernetes:

- `kubectl get services`: Lists all Services in the current namespace.
- `kubectl describe service <service-name>`: Provides detailed information about a specific Service.
- `kubectl delete service <service-name>`: Deletes a specific Service.
- `kubectl get svc <service-name> -o=jsonpath='{.status.loadBalancer.ingress[0].ip}'`: Retrieves the external IP address of a LoadBalancer service.
- `kubectl get svc <service-name> -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}'`: Retrieves the hostname of a LoadBalancer service.
- `kubectl top services`: Monitors the resource usage of services in the cluster.
- `kubectl run --rm -i --tty debug --image=busybox --restart=Never -- /bin/sh`: Runs a temporary container for debugging purposes.
.  - Inside the debug container:
    - `nslookup <service-name>`: Performs a DNS lookup for the service.
    - `wget <service-name>`: Checks if you can access the service.
- `kubectl expose deployment <deployment-name> --type=NodePort --name=<service-name>`: Exposes a deployment as a service with a NodePort type.
- `kubectl get endpoints <service-name>`: Lists endpoints (Pod IPs) associated with a service.

These commands are essential for managing, troubleshooting, and interacting with Services in Kubernetes.


### Important:

- Configure your AKS cluster's security groups or network policies to allow access to your Azure Cache for Redis if needed.
- Ensure that your AKS cluster can resolve the DNS name of your Azure Cache for Redis instance.
- Consider implementing environment variables or secrets management for sensitive information.

This is a basic example showcasing the concept. Remember to adjust it based on your specific deployment requirements and security best practices.

**Source:** [abhinavk1492/jenkins-flask-tutorial](https://github.com/abhinavk1492/jenkins-flask-tutorial)

This GitHub repository contains the source code for a Jenkins-Flask tutorial. It likely includes files and resources related to setting up Jenkins and Flask applications, along with tutorials or documentation on how to use them together.

You can visit the repository by following the link: [abhinavk1492/jenkins-flask-tutorial](https://github.com/abhinavk1492/jenkins-flask-tutorial)
```

## 3. ReplicaSets: Scaling Your Pods

Imagine a ReplicaSet as a landlord managing identical apartments (Pods) in a building. It ensures a desired number of identical Pods are running at all times. If a Pod gets evicted (fails), the ReplicaSet creates a new one to maintain occupancy.
```bash
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-nginx-replicaset
spec:
  replicas: 3 # Desired number of Pods (apartments)
  selector:
    matchLabels:
      app: nginx # Selects Pods with label "app: nginx"
  template:
    metadata:
      labels:
        app: nginx # Label applied to Pods created by the ReplicaSet
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```
### Useful Commands for ReplicaSets

Here are some useful commands for interacting with ReplicaSets in Kubernetes:

- `kubectl get replicasets`: Lists all ReplicaSets in the current namespace.
- `kubectl describe replicaset <replicaset-name>`: Provides detailed information about a specific ReplicaSet.
- `kubectl scale replicaset <replicaset-name> --replicas=<number>`: Scales the number of replicas in a ReplicaSet.
- `kubectl delete replicaset <replicaset-name>`: Deletes a specific ReplicaSet.
- `kubectl edit replicaset <replicaset-name>`: Opens the specified ReplicaSet in a text editor, allowing you to modify its configuration interactively.
- `kubectl rollout status replicaset <replicaset-name>`: Checks the status of a rolling update for a ReplicaSet.
- `kubectl rollout history replicaset <replicaset-name>`: Displays the rollout history of changes to a ReplicaSet, including revisions and their status.
- `kubectl rollout undo replicaset <replicaset-name>`: Rolls back a ReplicaSet to a previous revision.

These commands are essential for managing and troubleshooting ReplicaSets in Kubernetes.
## 4. Deployments: Managing Pod Lifecycles

Think of a deployment as an upgraded landlord who manages your pod lifecycles. Deployments ensure a desired number of pod replicas are running, handle scaling (adding or removing replicas), and facilitate rollouts and rollbacks of updates.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app-deployment
spec:
  replicas: 3  # Maintain 3 replicas
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```
## Useful Commands for Deployments

Here are some useful commands for interacting with Deployments in Kubernetes:

- `kubectl get deployments`: Lists all Deployments in the current namespace.
- `kubectl describe deployment <deployment-name>`: Provides detailed information about a specific Deployment.
- `kubectl scale deployment <deployment-name> --replicas=<number>`: Scales the number of replicas in a Deployment.
- `kubectl delete deployment <deployment-name>`: Deletes a specific Deployment.
- `kubectl edit deployment <deployment-name>`: Opens the specified Deployment in a text editor, allowing you to modify its configuration interactively.
- `kubectl rollout status deployment <deployment-name>`: Checks the status of a rolling update for a Deployment.
- `kubectl rollout history deployment <deployment-name>`: Displays the rollout history of changes to a Deployment, including revisions and their status.
- `kubectl rollout undo deployment <deployment-name>`: Rolls back a Deployment to a previous revision.
- `kubectl apply -f <deployment-file.yaml>`: Creates or updates a Deployment based on the configuration defined in a YAML file.
- `kubectl rollout pause deployment <deployment-name>`: Pauses a Deployment rollout, allowing you to investigate issues or perform maintenance.
- `kubectl rollout resume deployment <deployment-name>`: Resumes a paused Deployment rollout.
- `kubectl rollout restart deployment <deployment-name>`: Restarts a Deployment rollout, effectively redeploying all pods.
- `kubectl rollout status deployment <deployment-name> --watch`: Watches the status of a Deployment rollout in real-time.

These commands are essential for managing and troubleshooting Deployments in Kubernetes.

## Alright, see you in the next module.
