# Docker and Kubernetes Cheatsheet

This README provides a cheatsheet of essential Docker and Kubernetes commands for containerization, orchestration, and deployment. It covers commonly used commands, useful tips, and basic usage patterns.

---

## Table of Contents

- [Docker](#docker)
  - [Basic Commands](#basic-commands)
  - [Images](#images)
  - [Containers](#containers)
  - [Volumes](#volumes)
  - [Networking](#networking)
  - [Docker Compose](#docker-compose)
- [Kubernetes](#kubernetes)
  - [Basic Commands](#kubernetes-basic-commands)
  - [Pods](#pods)
  - [Deployments](#deployments)
  - [Services](#services)
  - [ConfigMaps and Secrets](#configmaps-and-secrets)
  - [Namespaces](#namespaces)
  - [kubectl Cheat Sheet](#kubectl-cheat-sheet)

---

## Docker

### Basic Commands

- **Version and Info**:
  ```bash
  docker --version        # Show Docker version
  docker info             # Show system-wide information
  ```

- **Login and Logout**:
  ```bash
  docker login            # Log in to Docker Hub
  docker logout           # Log out of Docker Hub
  ```

### Images

- **Pulling and Listing Images**:
  ```bash
  docker pull <image>     # Pull an image from a repository
  docker images           # List all local images
  ```

- **Building Images**:
  ```bash
  docker build -t <name:tag> .               # Build an image from a Dockerfile in the current directory
  docker build -f <path/to/Dockerfile> .     # Specify a custom Dockerfile path
  ```

- **Tagging and Removing Images**:
  ```bash
  docker tag <image> <repository:tag>        # Tag an image
  docker rmi <image-id>                      # Remove an image
  ```

### Containers

- **Creating and Running Containers**:
  ```bash
  docker create --name <name> <image>        # Create a container without starting it
  docker run -d --name <name> <image>        # Run a container in detached mode
  docker run -it <image> /bin/bash           # Run a container interactively
  ```

- **Managing Containers**:
  ```bash
  docker start <container>                   # Start a stopped container
  docker stop <container>                    # Stop a running container
  docker restart <container>                 # Restart a container
  docker rm <container>                      # Remove a container
  ```

- **Viewing Logs and Inspecting Containers**:
  ```bash
  docker logs <container>                    # View container logs
  docker inspect <container>                 # View detailed information about a container
  docker exec -it <container> /bin/bash      # Execute commands inside a running container
  ```

### Volumes

- **Creating and Managing Volumes**:
  ```bash
  docker volume create <volume_name>         # Create a volume
  docker volume ls                           # List all volumes
  docker volume rm <volume_name>             # Remove a volume
  ```

- **Mounting Volumes in Containers**:
  ```bash
  docker run -v <volume_name>:<path_in_container> <image>
  docker run -v $(pwd)/local_dir:/app <image>   # Mount local directory
  ```

### Networking

- **Networking Commands**:
  ```bash
  docker network ls                           # List networks
  docker network create <network_name>        # Create a new network
  docker network rm <network_name>            # Remove a network
  ```

- **Connecting Containers to a Network**:
  ```bash
  docker network connect <network_name> <container_name>
  docker run --network <network_name> <image>
  ```

### Docker Compose

- **Basic Docker Compose Commands**:
  ```bash
  docker-compose up                           # Start services defined in docker-compose.yml
  docker-compose up -d                        # Start services in detached mode
  docker-compose down                         # Stop and remove containers
  docker-compose ps                           # List all running services
  docker-compose logs                         # View logs for all services
  ```

---

## Kubernetes

### Kubernetes Basic Commands

- **Cluster Info and Context**:
  ```bash
  kubectl cluster-info                        # Display cluster info
  kubectl config get-contexts                 # List contexts
  kubectl config use-context <context-name>   # Switch context
  ```

- **Namespaces**:
  ```bash
  kubectl get namespaces                      # List all namespaces
  kubectl create namespace <namespace_name>   # Create a namespace
  kubectl delete namespace <namespace_name>   # Delete a namespace
  ```

### Pods

- **Creating and Managing Pods**:
  ```bash
  kubectl run <pod_name> --image=<image>      # Create a pod
  kubectl get pods                            # List all pods
  kubectl delete pod <pod_name>               # Delete a pod
  kubectl describe pod <pod_name>             # Describe a pod's details
  kubectl logs <pod_name>                     # View logs for a pod
  kubectl exec -it <pod_name> -- /bin/bash    # Execute a command inside a pod
  ```

### Deployments

- **Creating and Updating Deployments**:
  ```bash
  kubectl create deployment <name> --image=<image>
  kubectl apply -f <deployment.yaml>          # Apply a deployment YAML file
  kubectl rollout restart deployment <name>   # Restart deployment pods
  kubectl rollout undo deployment <name>      # Rollback to previous revision
  ```

- **Viewing Deployment Status**:
  ```bash
  kubectl get deployments                     # List deployments
  kubectl describe deployment <name>          # Get details of a deployment
  ```

### Services

- **Creating and Exposing Services**:
  ```bash
  kubectl expose pod <pod_name> --type=<type> --port=<port>
  kubectl expose deployment <deployment_name> --type=<type> --port=<port>
  kubectl get services                        # List all services
  kubectl delete service <service_name>       # Delete a service
  ```

  - **Service Types**:
    - `ClusterIP`: Internal access only (default)
    - `NodePort`: Exposes service on each Node’s IP
    - `LoadBalancer`: Exposes service externally via cloud provider’s load balancer

### ConfigMaps and Secrets

- **Creating ConfigMaps**:
  ```bash
  kubectl create configmap <config_name> --from-literal=key=value
  kubectl create configmap <config_name> --from-file=<file_path>
  kubectl get configmaps                      # List ConfigMaps
  ```

- **Creating Secrets**:
  ```bash
  kubectl create secret generic <secret_name> --from-literal=key=value
  kubectl get secrets                         # List all secrets
  kubectl describe secret <secret_name>       # Get details of a secret
  ```

### Namespaces

- **Using and Managing Namespaces**:
  ```bash
  kubectl config set-context --current --namespace=<namespace_name>
  kubectl get all --namespace=<namespace_name>
  ```

### kubectl Cheat Sheet

- **Common Commands**:
  ```bash
  kubectl get all                             # List all resources
  kubectl delete <resource_type> <resource_name>  # Delete resource
  kubectl apply -f <file.yaml>                # Apply a configuration from a file
  kubectl edit <resource_type> <resource_name>  # Edit an existing resource
  ```

- **Resource Types**:
  - Pods: `kubectl get pods`
  - Nodes: `kubectl get nodes`
  - Services: `kubectl get services`
  - Deployments: `kubectl get deployments`
  - StatefulSets: `kubectl get statefulsets`
  - ConfigMaps: `kubectl get configmaps`
  - Secrets: `kubectl get secrets`
  - Persistent Volumes: `kubectl get pv`
  - Persistent Volume Claims: `kubectl get pvc`

- **Troubleshooting Commands**:
  ```bash
  kubectl describe <resource_type> <resource_name>  # Describe a resource in detail
  kubectl logs <pod_name>                           # View logs of a pod
  kubectl logs <pod_name> -c <container_name>       # View logs of a specific container
  kubectl top pod <pod_name>                        # View resource usage of a pod
  ```

---
