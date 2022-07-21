# Installing k8s
* installing docker
```
curl -fsSL https://get.docker.com -o get-docker.sh
```
```
sh get-docker.sh
```
```
sudo usermod -aG docker ubuntu
```
```
sudo -i
```
```
# Run these commands as root
###Install GO###
```
wget https://storage.googleapis.com/golang/getgo/installer_linux
```
```
chmod +x ./installer_linux

./installer_linux

source ~/.bash_profile
```
```
git clone https://github.com/Mirantis/cri-dockerd.git
```
```
cd cri-dockerd

mkdir bin
```
```
go get && go build -o bin/cri-dockerd
```
```
mkdir -p /usr/local/bin
```
```
install -o root -g root -m 0755 bin/cri-dockerd /usr/local/bin/cri-dockerd
```
```
cp -a packaging/systemd/* /etc/systemd/system
```
```
sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service
```
```
systemctl daemon-reload

systemctl enable cri-docker.service

systemctl enable --now cri-docker.socket
```
# Execute these commands as normal user
```
sudo apt-get update
```
```
sudo apt-get install -y apt-transport-https ca-certificates curl
```
```
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
```
```
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
```
sudo apt-get update
```
```
sudo apt-get install -y kubelet kubeadm kubectl
```
```
sudo apt-mark hold kubelet kubeadm kubectl
```
# Exexcute the below command as Root User
```
 kubeadm init --pod-network-cidr=10.244.0.0/16 --cri-socket unix:///var/run/cri-dockerd.sock
 ```
 # Execute these commands as normal user
 ```
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

        
*********** Execute the below command ONLY IN MASTER ***********
 
 kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml