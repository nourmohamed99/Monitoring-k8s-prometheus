# Monitoring kubernetes cluster using prometheus 

In this project, we will install Docker, Kubernetes (minikube & kubectl), Helm, Prometheus, and Grafana from scratch on a centos 9 machine. 
Use Prometheus to monitor the Kubernetes cluster.

## Installation

open a terminal with administrator access.

Remove old verisons of Docker 

```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### Install Docker:  

1. update system
   
```
sudo yum update

```
2. Insatll ` yum-utils` package: 

```
sudo yum install -y yum-utils
```
3. Add Docker Repository:

```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
4. Install Docker Engine:

```
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
5. Start Docker service:

```
sudo systemctl enable --now docker
```
6. Verify Installation:
   
```
docker --version
```

### Install Minikube

First, ensure that you have the necessary prerequisites:

- 2 CPUs or more.
- 2GB or more of free memory.
- 20GB of free disk space.
- Internet connection.
- A container or virtual machine manager such as Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation.  

1. Install Kubelet:

```
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kubelet
```
2. Download Minikube binary:

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
```
3. Install Minikube:

```
sudo rpm -Uvh minikube-latest.x86_64.rpm
```
4. Start Minikube cluster:

```
minikube start --driver=docker --force
```
## Install Kubectl   

1. Download the latest release with the command:

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
```
To download a specific version, replace the

```
$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)
```
portion of the command with the specific version.
For example, to download version v1.7.0 on Linux, type:
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.7.0/bin/linux/amd64/kubectl
```
2. Make the kubectl binary executable:

```
chmod +x ./kubectl
```   
3. Move the binary in to your PATH:

```
sudo mv ./kubectl /usr/local/bin/kubectl
```
### Install Helm

1. Download Helm installation script:

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```
2. Provide execute permission to the script:

```
chmod 700 get_helm.sh
```
3. Run the Helm installation script:

```
./get_helm.sh
```

### Install and Deploy Prometheus

1. Add Prometheus Helm repository:

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
2. Update the Helm repositories:

```
helm repo update
```
3. Install Prometheus using Helm:

```
helm install prometheus prometheus-community/prometheus
```
4. Expose Prometheus service:

```
kubectl expose service prometheus-server — type=NodePort — target-port=9090 — name=prometheus-server-ext
```
5. Open Web App of Prometheus:

```
minikube service prometheus-server-ext
```


### Install and Deploy Grafana

1. Add Grafana Helm repository:

```
helm repo add grafana https://grafana.github.io/helm-charts
```
2. Update the Helm repositories:

```
helm repo update
```
3. Install Grafana using Helm: 

```
helm install grafana grafana/grafana
```
4. Expose Grafana service:

```
kubectl expose service grafana — type=NodePort — target-port=3000 — name=grafana-ext
```
5. Open Grafana Web APP

```
minikube service grafana-ext
```
6. Get your Admin user password by running and login with the password and username= admin:

```
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

```

### Monitoring cluster through Prometheus

- Select status ---> targets

![image](https://github.com/nourmohamed99/Monitoring-k8s-prometheus/assets/88977873/b7df6078-f6dd-4010-b4d4-cf44df32160b)

- Select Graph
- Run some queries to retrieve metric data about your new server:
```
up
```

![image](https://github.com/nourmohamed99/Monitoring-k8s-prometheus/assets/88977873/84665833-7e74-45e1-b3ea-42141dcf92e2)

### Monitoring cluster through Grafana


- Menu ---> Data Sources ---> Add new data source ---> prometheus
-  add prometheus server url ---> save & test

- Menu ---> Dashboards ---> Dashboards ---> import dashboard
- add url
```
3662
```
then load 

![image](https://github.com/nourmohamed99/Monitoring-k8s-prometheus/assets/88977873/ce79e522-37ff-4885-8fd0-dc1e46a66c25)
