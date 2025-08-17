Goal: Setup kubernetes infrastructure and manage locally for development.

Softwares required:
- Docker
- Minikube also called as cluster
- Kubectl 

Installing Docker:
sudo dnf -y update
sudo dnf -y install dnf-plugins-core
sudo tee /etc/yum.repos.d/docker-ce.repo<<EOF
[docker-ce-stable]
name=Docker CE Stable - \$basearch
baseurl=https://download.docker.com/linux/fedora/\$releasever/\$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/fedora/gpg
EOF
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo systemctl enable --now docker

Kubectl & Minikube installation on linux:
Kubectl installation: sudo dnf install -y kubectl (Kubectl has to be installed before minikube, otherwise minikube throws an error unable to find kubectl)
Download minikube: curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
Install minikube: sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

Start minikube or cluster: minikube start --driver=docker
- We can run minikube without setting any driver also. But without setting driver we won't be able to reach endpoints exposed in browser. Hence it is very important to set driver here and I have chosen to use docker for convenience.

Build the docker image: docker build -t anjanipiratla/kub-first-app .

Push image to docker hub: docker push anjanipiratla/kub-first-app (I use locally docker desktop. When I am logged in to docker desktop using my credentials it allows me to push the image to docker hub without any probelms).

Visit cluster dashboard by: minikube dashboard (This will open kubernete dashboard locally in your default browser)

Create a deployment to cluster: kubectl create deployment first-app --image=anjanipiratla/kub-first-app

Exposing a service as loadbalancer: kubectl expose deployment first-app --type=LoadBalancer --port=8080

minikube makes it easy to open this exposed endpoint in your browser: minikube service first-app

We can scale the pods by using dashboard.

If any changes in code, then we have to build the image and push to docker. When pushing image to docker we need to give it a tag so that when deploy the image the changes will show up otherwise changes will not show up.

Pushing rebuilt image to docker hub: docker push anjanipiratla/kub-first-app:2

Re-create deployment: kubectl set image deployment first-app kub-first-app=anjanipiratla/kub-first-app:2

Stop minikube or cluster: minikube stop







