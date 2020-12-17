## Setup

```sh
# Initialize gcloud CLI
gcloud init

# List all projects
gcloud projects list

# Setup gcloud cred helper (will allow us to push docker images to google cloud registry)
gcloud auth configure-docker

# Push image to Google Container Registry
# <hostname>/<project-id>/<image-name>:<tag>
docker build -t gcr.io/demo-project-123/demo:1.0 .
docker push gcr.io/demo-project-123/demo:1.0
```

hostname - host for google container registry that will store image
project-id - GCP project to use
image-name - name of the image
tag - tag for the image

## Create Cluster and Service
```sh
# Create cluster (https://cloud.google.com/kubernetes-engine/docs/how-to/creating-a-regional-cluster)
# Creating the cluster changes the kubernetes context
gcloud container clusters create demo-cluster --num-nodes=3 \
--zone=asia-northeast3-a

# Get nodes (will show the 3 nodes created)
kubectl get nodes

# Create deployment
kubectl create deployment demo-app --image=gcr.io/demo-project-123/demo:1.0

# Expose deployment
kubectl expose deployment demo-app \
--type=LoadBalancer --port 5000 --target-port 5000
```