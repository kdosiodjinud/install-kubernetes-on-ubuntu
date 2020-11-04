# install-kubernetes-on-ubuntu

## From VPS with Ubuntu

```sh
$ apt update
$ apt upgrade
$ do-release-upgrade

$ reboot

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
$ add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

$ cat <<EOF | tee /etc/apt/sources.list.d/kubernetes.list \
deb https://apt.kubernetes.io/ kubernetes-xenial main \
EOF

$ swapoff -a // maybe edit /etc/fstab

$ apt -y install apt-transport-https ca-certificates software-properties-common curl kubelet kubeadm kubectl docker-ce kubernetes-cni

$ apt-mark hold kubelet kubeadm kubectl kubernetes-cni

$ vim /lib/systemd/system/docker.service //edit: ExecStart=/usr/bin/dockerd --exec-opt native.cgroupdriver=systemd

$ systemctl daemon-reload
$ systemctl restart docker

$ echo "symfony-vps.reddwarf.cloud" > /etc/hostname

$ kubeadm init --pod-network-cidr=192.168.0.0/16 --control-plane-endpoint "89.221.220.50:6443"

$ kubectl taint nodes --all node-role.kubernetes.io/master-
$ echo "kubectl taint nodes --all node-role.kubernetes.io/master-" >> /etc/sysctl.conf

$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

$ snap install helm --clasic
$ helm repo add haproxy-ingress https://haproxy-ingress.github.io/charts 
$ helm install haproxy-ingress haproxy-ingress/haproxy-ingress --create-namespace --namespace=ingress-controller --set controller.hostNetwork=true

## From local machine
$ scp root@your-master-ip-here:/etc/kubernetes/admin.conf ~/.kube/config
