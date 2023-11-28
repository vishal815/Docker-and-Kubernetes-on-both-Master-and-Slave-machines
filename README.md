Commands for setting up Docker and Kubernetes on both Master and Slave machines, along with some additional commands for deploying and managing Kubernetes resources. If you want to create a GitHub README file, you can format the provided information for better readability. Here's a simple template that you can use for your README file:

```markdown
# Kubernetes Installation and Configuration Guide

This guide provides step-by-step instructions for setting up Docker and Kubernetes on both Master and Slave machines. Additionally, it includes commands for deploying custom pods and managing Kubernetes resources.

## Prerequisites

- Ubuntu-based operating system
- Internet connection

## Docker Installation

```bash
# Common for both Master and Slave Machine
sudo apt-get update && apt-get install -y curl apt-transport-https
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install containerd.io docker-ce
```

## Kubernetes Installation

```bash
# Common for both Master and Slave Machine
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" >/etc/apt/sources.list.d/kubernetes.list
apt-get update
apt -y install kubeadm kubectl kubelet

# Open Below file and then comment disabled_plugin for CRI Runtime
vi /etc/containerd/config.toml    # Add # in front of disabled_plugin in this file and save it
service containerd restart
```

## Kubernetes Master Node Configuration

```bash
# Limited to Master Node
kubeadm init
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubeadm token create --print-join-command
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

## Deploying Custom Pods

```bash
# Using Below Commands to Deploy Custom PODS
kubectl get node
kubectl get pods --all-namespaces
kubectl taint nodes --all node-role.kubernetes.io/master-
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
kubectl get pods --all-namespaces

kubectl create deployment kubernetes-bootcamp --image=docker.io/jocatalin/kubernetes-bootcamp:v1 --port=8080
kubectl get deployments
kubectl get pods
kubectl describe pod kubernetes-bootcamp-7dc9765bf6-gbkjw
kubectl exec -ti $POD_NAME curl localhost:8080

kubectl get services

kubectl expose deployment/kubernetes-bootcamp --port=8080 --target-port=8080 --type=NodePort

kubectl describe services kubernetes-bootcamp

kubectl scale deployments/kubernetes-bootcamp --replicas=2

kubectl get pods -o wide

kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2

kubectl rollout status deployments/kubernetes-bootcamp

kubectl rollout undo deployments/kubernetes-bootcamp
```

For more details and updates, refer to [InstallationGuides/Kubernetes.txt](https://github.com/anujdevopslearn/InterviewQuestions/blob/master/InstallationGuides/Kubernetes.txt).
```

Copy and paste this template into your GitHub README file. Make sure to replace any placeholders with actual information if needed.
