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