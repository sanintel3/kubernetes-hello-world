# What is this repo for?
This repo provides helm charts to deploy ngnix hello world app to kuberntes cluster.

# Prerequisites
Your dev machine should have below packages installed
* docker, to install please follow this [page](https://docs.docker.com/engine/install/)
* helm, to install please follow this [page](https://helm.sh/docs/intro/install/)
* minikube, to install please follow this [page](https://minikube.sigs.k8s.io/docs/start/)

# Steps to deploy the application
* Execute `minikube start` to create kubernetes cluster
* Exeucute `minikube addons enable metrics-server`to enable metrics server which is required for HPA
* Clone this repo using git clone
* Execute `cd deploy/helm`
* Execute `helm install --create-namespace --namespace hello-world release ./hello-world`. This will deploy the application and create various kubernetes objects like deployment/service/hpa/pdb etc.
* When any changes are made to kubernetes manifests, execute `helm upgrade release ./hello-world --namespace hello-world` to deploy and upgrade the application to a new revision

# How to monitor/view application and deployment
* Execute `minikube dashboard`. This will open dashboard in a browser and change the namespace to 'hello-world' from top dropdown box, hello-world application health and various other kubernetes objects can be viewed from this dashboard
* To view application, execute `minikube service hello-world-release -n hello-world` this will open ngnix home page in a browser
* To view API_KEY, login to pod using commands
1. `kubectl get pods -n hello-world`
2. `kubectl exec --stdin --tty hello-world-release-<unique-id> -- /bin/sh`. This should get the user into the pod
3. Execute `env | grep API_KEY` to view the secret value

# Pending tasks
* Currently API_KEY value is not encrypted and base64 encoded value is checked-in to the repository, which is not an ideal practice. There are best practices to deal with this like using helm secrets plugin or use hashicorp vault to manage secrets. Also for better security, encryption at rest can be enabled on kubernetes side as well
