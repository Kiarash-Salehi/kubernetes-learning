To build the Docker image locally and use it without pushing it to Docker Hub, you can directly refer to the locally built image in your Kubernetes configuration. Here's the step-by-step guide for this scenario:

### 1. Write a Simple Golang Server

Create a simple Golang server that listens on port 8080. Save this file as `main.go`.

```go
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, Kubernetes!")
}

func main() {
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8080", nil)
}
```

### 2. Create a Dockerfile

Create a `Dockerfile` to containerize your Golang application. Save this file as `Dockerfile`.

```Dockerfile
# Start from the official Golang base image
FROM golang:1.18

# Set the Current Working Directory inside the container
WORKDIR /app

# Copy go mod and sum files
COPY go.mod go.sum ./

# Download all dependencies. Dependencies will be cached if the go.mod and go.sum files are not changed
RUN go mod download

# Copy the source code into the container
COPY . .

# Build the Go app
RUN go build -o main .

# Expose port 8080 to the outside world
EXPOSE 8080

# Command to run the executable
CMD ["./main"]
```

### 3. Build the Docker Image Locally

Build the Docker image locally using the `docker build` command.

```sh
# Build the Docker image
docker build -t go-server:local .
```

### 4. Create Kubernetes Deployment Configuration

Modify the `deployment.yaml` file to refer to the locally built Docker image.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: go-server-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: go-server
  template:
    metadata:
      labels:
        app: go-server
    spec:
      containers:
        - name: go-server
          image: go-server:local
          imagePullPolicy: Never
          ports:
            - containerPort: 8080
```

### 5. Create Kubernetes Service Configuration

Create a `service.yaml` file to expose your Deployment.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: go-server-service
spec:
  selector:
    app: go-server
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

### 6. Set Up Minikube or Kind for Local Kubernetes Cluster

To use a local Kubernetes cluster, you can use tools like Minikube or Kind (Kubernetes IN Docker). Here, we'll use Minikube as an example.

Install Minikube if you haven't already:

```sh
# For Linux
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# For macOS
brew install minikube

# For Windows
choco install minikube
```

Start Minikube:

```sh
minikube start
```

### 7. Load the Docker Image into Minikube

Minikube runs its own Docker daemon, so you need to load your local image into Minikube's Docker environment.

```sh
minikube image load go-server:local
```

### 8. Deploy to Kubernetes

Deploy the application to your Minikube cluster using the `kubectl` command-line tool.

```sh
# Apply the Deployment configuration
kubectl apply -f deployment.yaml

# Apply the Service configuration
kubectl apply -f service.yaml
```

### 9. Verify the Deployment

Check the status of your Deployment and Service to ensure everything is running correctly.

```sh
# Get the status of the Deployment
kubectl get deployments

# Get the status of the Pods
kubectl get pods

# Get the status of the Service
kubectl get services
```

### 10. Access the Application

Once the Service is up and running, you can access your Golang server using the Minikube service command to get the URL.

```sh
minikube service go-server-service
```

This command will open your default web browser and navigate to the URL of your Golang server, where you should see "Hello, Kubernetes!".

This completes the detailed steps to deploy a simple Golang server using a local Kubernetes cluster with Minikube.
