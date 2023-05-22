# Installing Minikube with Docker on Ubuntu 20.04 Behind a Proxy

# Step 1: Install Docker

```
sudo su -
apt update 
apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt install docker-ce
```

```nano /usr/lib/systemd/system/docker.service :```

```
[Service]
Environment="HTTP_PROXY=<IP-PROXY>:<PORT>"
Environment="HTTPS_PROXY=<IP-PROXY>:<PORT>"
Environment="NO_PROXY="localhost,127.0.0.1,::1"
```

```exit ```


```
sudo systemctl daemon-reload
sudo systemctl restart docker.service
sudo usermod -aG docker $USER && newgrp docker
```

# Step 2: Install Minikube and kubectl
## Install Minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
``````


## Start Minikube with Docker driver

```minikube start --docker-env http_proxy=$http_proxy --docker-env https_proxy=$https_proxy```


export no_proxy=$no_proxy,$(minikube ip)
export no_proxy=$no_proxy,$(minikube ip)


# Step 3 Install and kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

# Verify kubectl installation
```kubectl version --client```

# Conclusion
```echo "Minikube and kubectl installation completed successfully."```

