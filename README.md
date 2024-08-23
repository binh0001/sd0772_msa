# sd1121_msa

MSA application for Practical Devops assignment

Jenkins
Install plugins

- Docker Pipeline
- Pipeline Utility Steps
- Kubernetes
- Amazon EC2
- Pipeline: AWS Steps

Credentials

- aws-credential
- ubuntu-user

Add Amazon EC2 to Jenkins clouds

Amazon EC2

# setup aws cli

- curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
- unzip awscliv2.zip
- sudo ./aws/install

# setup kubectl

- curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm64/kubectl"
  or
- curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

- sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
- chmod +x kubectl
- mkdir -p ~/.local/bin
- mv ./kubectl ~/.local/bin/kubectl

# Fix docker

- sudo chmod 666 /var/run/docker.sock

# Port forward

- kubectl port-forward svc/frontend 3000:3000

# setup argocd

- kubectl create namespace argocd
- kubectl create namespace app-argocd
- kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
- kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath=”{.data.password}”
- kubectl port-forward svc/argocd-server -n argocd 8084:443

# Prometheus and Grafana

- helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
- helm repo add stable https://charts.helm.sh/stable
- helm repo update
- helm install prometheus prometheus-community/kube-prometheus-stack
- kubectl port-forward service/prometheus-grafana 8082:80

- username: admin
- password: prom-operator
- https://grafana.com/grafana/dashboards/
