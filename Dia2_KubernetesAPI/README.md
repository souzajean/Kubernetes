# Current context

kubectl config current-context


# List all contexts

kubectl config get-contexts


# Change context

kubectl config use-context [contextName]


# Using kubectx
## What’s great about Kubernetes is the incredible amount of tools created by the community and available for free. Kubectx is a simple tool that provides an easy way to list and change context.

## You can install it on:

### Windows (if you have Chocolatey installed):

choco install kubectx-ps
### macOS (if you have Brew installed):

brew install kubectx
### Ubuntu:

sudo apt install kubectx
## To list the contexts, simply type:

kubectx
### To change context:

kubectx <contextName>


# Rename context
Rename context:

kubectl config rename-context [old-name] [new-name]


# Delete context
Delete context:

kubectl config delete-context [contextName]


# The-declarative-way-vs-the-imperative-way

## Comandos
kubectl run mynginx --image=nginx -- port=80 <br>
kubectl create deploy mynginx --image=nginx --port=80 replicas=3 <br>
kubectl create service noport myservice --targetPort=8080 <br>
kubectl delete pod nginx

## YAML
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: front-end
spec:
  containers:
    - name: nginx-container
    image: nginx    

# kubectl create deploy mynginx --image=nginx --port=80 replicas=3 --dry-run=client -o yaml
