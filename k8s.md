. Подготовить систему к установке Kubernetes

# Настроить Cgroup drivers для Docker CE

sudo vi /etc/docker/daemon.json

{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}

# Выключить swap

sudo swapoff -a

# Настроить SELinux, если включен

sudo setenforce 0 sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

# Установка tc

sudo yum install tс

# Если используется iptables, разрешить ему видеть bridged трафик

sudo vi /etc/sysctl.d/k8s.conf

net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
sysctl --system
---
    1  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    2  sudo halt
    3  sudo nano /etc/hosts
    4  sudo apt -y install curl apt-transport-https \\n&& curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add - \\n&& echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list \\n&& sudo apt update \\n&& sudo apt -y install vim git curl wget kubelet kubeadm kubectl \\n&& sudo apt-mark hold kubelet kubeadm kubectl \\n&& kubectl version --client && kubeadm version
    5  df -h
    6  top
    7  df -h
    8  cat /etc/fstab
    9  sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab \\n&& sudo swapoff -a \\n&& sudo modprobe overlay \\n&& sudo modprobe br_netfilter
   10  cat /etc/fstab  
   11  sudo tee /etc/sysctl.d/kubernetes.conf<<EOF\nnet.bridge.bridge-nf-call-ip6tables = 1\nnet.bridge.bridge-nf-call-iptables = 1\nnet.ipv4.ip_forward = 1\nEOF  
   12  cat /etc/sysctl.d/kubernetes.conf  
   13  sudo sysctl --system  
   14  sudo apt update \\n&& sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates \\n&& curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - \\n&& sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \\n&& sudo apt update \\n&& sudo apt install -y containerd.io docker-ce docker-ce-cli \\n&& sudo usermod -aG docker ambr \\n&& sudo mkdir -p /etc/systemd/system/docker.service.d
   15  sudo tee /etc/docker/daemon.json <<EOF\n{\n  "exec-opts": ["native.cgroupdriver=systemd"],\n  "log-driver": "json-file",\n  "log-opts": {\n    "max-size": "100m"\n  },\n  "storage-driver": "overlay2"\n}\nEOF
   16  cat /etc/docker/daemon.json
   17  sudo systemctl daemon-reload \ \n&& sudo systemctl restart docker \\n&& sudo systemctl enable docker
   18  reboot
   19  sudo reboot
   20  lsmod | grep br_netfilter
   21  lsmod | grep overlay
   22  sudo systemctl enable kubelet
   23  sudo kubeadm config images pull
   24  sudo systemctl status smbd
   25  sudo kubeadm init --apiserver-advertise-address 192.168.111.8 --pod-network-cidr=10.225.0.0/16 --control-plane-endpoint=kubernetes
   26  mkdir -p $HOME/.kube \\n&& sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config \\n&& sudo chown $(id -u):$(id -g) $HOME/.kube/config
   27  mc
   28  kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
   29  kubectl taint nodes --all node-role.kubernetes.io/master-
   30  kubectl logs --namespace=kube-system -l k8s-app=kube-dns
   31  kubectl get all
   32  kubectl get all --all-namespaces
   33  watchkubectl get all --all-namespaces
   34  watch kubectl get all --all-namespaces
   35  kubectl logs --namespace=kube-system -l k8s-app=kube-dns
   36  kubectl apply -f https://k8s.io/examples/admin/dns/dnsutils.yaml
