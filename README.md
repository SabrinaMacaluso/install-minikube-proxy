# Installing Minikube with Docker on Ubuntu 20.04 Behind a Proxy

# Step 1: Install Docker

```bash
sudo su -
apt update 
apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt install docker-ce
```

```
nano /usr/lib/systemd/system/docker.service :
```

```
[Service]
Environment="HTTP_PROXY=<IP-PROXY>:<PORT>"
Environment="HTTPS_PROXY=<IP-PROXY>:<PORT>"
Environment="NO_PROXY="localhost,127.0.0.1,::1"
```

```bash
  exit 
  ```


```bash
sudo systemctl daemon-reload
sudo systemctl restart docker.service
sudo usermod -aG docker $USER && newgrp docker
```

# Step 2: Install Minikube
## Install Minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
``````


## Start Minikube with Docker driver

```bash
minikube start --docker-env http_proxy=$http_proxy --docker-env https_proxy=$https_proxy
```

 Add the following lines to ~./bashrc file:
 
```bash
export no_proxy=$no_proxy,$(minikube ip)
export NO_PROXY=$no_proxy,$(minikube ip)
```
then reload the .bashrc file:

```bash
source ~./bashrc
```

# Step 3: Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

# Verify kubectl installation
```bash
kubectl version --client
```

# Conclusion
```bash 
echo "Minikube and kubectl installation completed successfully."
```

# Troubleshooting

If you encounter the error message "Failed to restart docker.service: Unit docker.service is masked."

1- Add User to Docker group and start minikube again:

```bash
sudo usermod -aG docker $USER && newgrp docker
minikube start --docker-env http_proxy=$http_proxy --docker-env https_proxy=$https_proxy
```

2- Try to switch to User account, run the following command to switch account and start minikube again: 

```bash
sudo su - <your_username>
minikube start --docker-env http_proxy=$http_proxy --docker-env https_proxy=$https_proxy
```

3- Check Docker service file "/usr/lib/systemd/system/docker.service" and verify if the proxy is still present.


