## Install Container Runtime (containerd) [ CentOS / RHEL ]
**This step needs to be followed on all nodes [Leader and Follower]**
* Steps to install containerd in [CentOS/RHEL]

```
dnf install -y  yum-utils device-mapper-persistent-data lvm2

dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo

dnf update -y && dnf install -y containerd.io

mkdir -p /etc/containerd

containerd config default > /etc/containerd/config.toml# Restart containerd 

systemctl restart containerd

systemctl enable containerd

systemctl status containerd
```

* Download `crictl` executable for checking the successfull status of containerd | [Official Documentation](https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md)
```
VERSION="v1.26.0" 
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/$VERSION/crictl-$VERSION-linux-amd64.tar.gz
sudo tar zxvf crictl-$VERSION-linux-amd64.tar.gz -C /usr/local/bin
rm -f crictl-$VERSION-linux-amd64.tar.gz
```

* Add the below lines in the file: `/etc/crictl.yaml`
```
runtime-endpoint: unix:///var/run/containerd/containerd.sock
image-endpoint: unix:///var/run/containerd/containerd.sock
timeout: 2
```
* Run `crictl images` and  `crictl ps -a` to ensure containerd is successfully runing.
