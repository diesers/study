### 关闭selinux
```sh
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
```
### 关闭swap
使用free指令查看系统swap是否有开启，如果开启了，编辑 /etc/fstab，直接删除掉swap列
### 给docker分配独立硬盘,在如果只有一块硬盘不用设置。一般为了安全和方便扩容等考虑，会另外划分一个或多个数据盘，供容器使用。
```sh
mkdir -p /var/lib/docker
fdisk -l
fdisk /dev/vdb     # n p 3个回车 w
mkfs.ext4 /dev/vdb1 
cat << EOF >> /etc/fstab
/dev/vdb1 /var/lib/docker ext4 defaults 1 1
EOF
mount -a
```
### 内核参数修改
```sh
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```
### 防火墙端口开放
主节点(Master)
主节点(Master)

协议|方向|端口范围|用途|使用者
:--|:--|:--|:--|:--
TCP|入方向|6443|k8s API server | 所有
TCP|入方向|2379-2380|etcd|kube-apiserver, etcd
TCP|入方向|10250|kubelet|自己
TCP|入方向|10251|kube-scheduler|自己
TCP|入方向|10252|kube-controller-manager|自己

工作节点(Worker)

协议|方向|端口范围|用途|使用者
:--|:--|:--|:--|:--
TCP|入方向|10250|kubelet|自己
TCP|入方向|30000-32767|Service|所有

一般生产环境可以简单的关闭防火墙来开放接口，最后通过云管理平台来约束物理节点的端口访问
```bash
service firewalld stop
systemctl disable firewalld
```
### 安装 kubeadm, kubelet, kubectl
- `kubeadm`: 用来创建集群的工具，随k8s版本升级，易用性会越来越强
- `kubelet`: 以服务模式运行，可以理解为k8s在物理节点上的代理，k8s通过它来控制分配在节点上的所有容器
- `kubectl`: 日常和k8s对话的工具


```bash
# 设置一个临时环境变量PROXY，方便下面的脚本配置，小技巧，实际操作请换成自己的实际代理IP
export PROXY=http://192.168.0.127:8118/

# yum加代理
cat << EOF >> /etc/yum.conf
proxy=$PROXY
EOF

# docker加代理
mkdir /etc/systemd/system/docker.service.d
cat << EOF > /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=$PROXY"
EOF

# 创建k8s.repo
cat << EOF > /etc/yum.repos.d/k8s.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

# 安装

yum install -y docker-ce kubelet kubeadm kubectl
systemctl enable docker kubelet
```

> 奇淫技巧：镜像国内加速(不保证能加速到最新版的k8s)

```sh
cat <<EOF > /etc/yum.repos.d/k8s.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

cat >/etc/sysconfig/kubelet<<EOF
KUBELET_EXTRA_ARGS="--pod-infra-container-image=registry.cn-hangzhou.aliyuncs.com/google_containers/pause-amd64:3.1"
EOF
```
修改主机名，便于识别
```sh
hostnamectl set-hostname master
```
列出需要拉取的镜像
```sh
kubeadm config images list
```
由于墙的原因，国内很可能拉取不到某些镜像。
我们可以用阿里云或者其他地方的源来代替。
一般公司会有一个镜像仓库，如果没有，可以建一个。或者使用云平台的镜像仓库。将镜像存储到镜像仓库里面，以便后续使用。
```sh
cat <<EOF > kubeadm-config.yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
kubernetesVersion: stable
imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers
networking:
    podSubnet: "10.244.0.0/16"
EOF
```
```sh
kubeadm config images pull --config kubeadm-config.yaml
```
```sh
kubeadm init --config kubeadm-config.yaml
```

部署dashboard
```sh
kubectl apply -f dashboard.yaml
kubectl get all -n kubernetes-dashboard 
kubectl describe secrets -n kubernetes-dashboard dashboard-admin
```
通过nodeip+port的方式访问dashboard
此处是https://192.168.10.181:30001
