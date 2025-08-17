# Kubernetes Node.js Setup Guide

## Goal

Set up Kubernetes infrastructure and manage it locally for development.

---

## Software Required

- **Docker**
- **Minikube** (cluster)
- **Kubectl**

---

## Step 1: Install Docker (Fedora)

sudo dnf -y update \
sudo dnf -y install dnf-plugins-core \
sudo tee /etc/yum.repos.d/docker-ce.repo <<EOF \
[docker-ce-stable] \
name=Docker CE Stable - $basearch \
baseurl=https://download.docker.com/linux/fedora/\$releasever/\$basearch/stable \
enabled=1 \
gpgcheck=1 \
gpgkey=https://download.docker.com/linux/fedora/gpg \
EOF \
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y \
sudo systemctl enable --now docker \


---

## Step 2: Install Kubectl & Minikube (Linux)

> **Install Kubectl first to avoid errors with Minikube.**

Install Kubectl
sudo dnf install -y kubectl

Download Minikube
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64

Install Minikube
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

---

## Step 3: Start Minikube Cluster

minikube start --driver=docker

- Setting the driver ensures you can reach endpoints exposed in the browser. Docker is recommended for convenience.

---

## Step 4: Build and Push the Docker Image

Build Docker image
docker build -t anjanipiratla/kub-first-app .

Push image to Docker Hub
docker push anjanipiratla/kub-first-app

---

## Step 5: Kubernetes Dashboard

minikube dashboard

- This opens the Kubernetes dashboard locally in your default browser.

---

## Step 6: Deploy Application to Cluster

kubectl create deployment first-app --image=anjanipiratla/kub-first-app

---

## Step 7: Expose Service as LoadBalancer

kubectl expose deployment first-app --type=LoadBalancer --port=8080

- Easily open the exposed endpoint in your browser: minikube service first-app


---

## Step 8: Scale Pods

- Use the dashboard to scale pods as needed.

---

## Step 9: Update the Application

- If you change the code: Build and push the Docker image with a new tag to Docker Hub.  
- Push rebuilt image with a new tag: docker push anjanipiratla/kub-first-app:2
- Update deployment to use new image: kubectl set image deployment first-app kub-first-app=anjanipiratla/kub-first-app:2

---

## Step 10: Stop Cluster

minikube stop

---




