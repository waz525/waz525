# 1 K8s 安装

## 1.1 环境准备

### 1.1.1 阿里云环境
| 公网IP        | 私网IP         | 用户名 | 端口 | 密码         |
| ------------- | -------------- | ------ | ---- | ------------ |
| 112.124.11.65 | 172.20.151.134 | root   | 22   | Fhaw****@65# |
| 120.26.82.188 | 172.20.151.135 | root   | 22   | Fhaw****@188# |
| 121.43.153.211| 172.20.151.136 | root   | 22   | Fhaw****@211# |

### 1.1.2 集群网段划分原则

1. 主机节点网段  172.20.151.0/24
2. Service网段     10.96.0.0/16
3. Pod网段           10.244.0.0/16

### 1.1.3 Github

Github账号：ydsl01  ydsl01@163.com  wqy**********

## 1.2 安装配置

### 1.2.1 配置hosts	

```shell
echo "
172.20.151.134 k8s-master01
172.20.151.134 k8s-master-lb
172.20.151.135 k8s-node01
172.20.151.136 k8s-node02
" >> /etc/hosts
```

### 1.2.2 配置yum源

```shell
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo

```

### 1.2.3 安装必备工具和服务器配置

```shell
#必备工具安装
yum install wget jq psmisc vim net-tools telnet yum-utils device-mapper-persistent-data lvm2 git -y



#所有节点关闭防火墙、selinux、dnsmasq、swap。服务器配置如下：
systemctl disable --now firewalld
systemctl disable --now dnsmasq
systemctl disable --now NetworkManager
setenforce 0
sed -i 's#SELINUX=enforcing#SELINUX=disabled#g' /etc/sysconfig/selinux
sed -i 's#SELINUX=enforcing#SELINUX=disabled#g' /etc/selinux/config
 

#关闭swap分区
swapoff -a && sysctl -w vm.swappiness=0
sed -ri '/^[^#]*swap/s@^@#@' /etc/fstab


#安装ntpdate
#rpm -ivh http://mirrors.wlnmp.com/centos/wlnmp-release-centos.noarch.rpm
yum install ntpdate -y

 
#所有节点同步时间。时间同步配置如下：

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
echo 'Asia/Shanghai' >/etc/timezone
systemctl stop ntpd
ntpdate time2.aliyun.com

# 加入到crontab
echo '
*/5 * * * * /usr/sbin/ntpdate time2.aliyun.com
' >> /etc/crontab 
systemctl restart crond


#所有节点配置limit：
ulimit -SHn 65535
# /etc/security/limits.conf末尾添加如下内容
echo '
* soft nofile 65536
* hard nofile 131072
* soft nproc 65535
* hard nproc 655350
* soft memlock unlimited
* hard memlock unlimited
' >> /etc/security/limits.conf
 
# 机器信任
ssh-keygen -t rsa
for i in k8s-master01 k8s-node01 k8s-node02;do ssh-copy-id -i .ssh/id_rsa.pub $i;done
```

### 1.2.4 下载安装所有的源码文件

```shell
cd /root/ ; git clone https://github.com/dotbalo/k8s-ha-install.git
#如果无法下载就下载：https://gitee.com/dukuan/k8s-ha-install.git

yum update -y --exclude=kernel*

reboot
```

### 1.2.5 升级内核

CentOS7 需要升级内核至4.18+，本地升级的版本为4.19

```shell
#在master01节点下载内核
cd /root
wget http://193.49.22.109/elrepo/kernel/el7/x86_64/RPMS/kernel-ml-devel-4.19.12-1.el7.elrepo.x86_64.rpm
wget http://193.49.22.109/elrepo/kernel/el7/x86_64/RPMS/kernel-ml-4.19.12-1.el7.elrepo.x86_64.rpm

# 从master01节点传到其他节点
for i in k8s-node01 k8s-node02;do scp kernel-ml-4.19.12-1.el7.elrepo.x86_64.rpm kernel-ml-devel-4.19.12-1.el7.elrepo.x86_64.rpm $i:/root/ ; done

#所有节点安装内核
cd /root && yum localinstall -y kernel-ml*

#所有节点更改内核启动顺序
grub2-set-default  0 && grub2-mkconfig -o /etc/grub2.cfg
grubby --args="user_namespace.enable=1" --update-kernel="$(grubby --default-kernel)"

# 检查默认内核是不是4.19
[root@k8s-node01 ~]# grubby --default-kernel
/boot/vmlinuz-4.19.12-1.el7.elrepo.x86_64
[root@k8s-node01 ~]#

# 所有节点重启，然后检查内核是不是4.19
[root@k8s-master01 ~]# uname -a
Linux k8s-master01 4.19.12-1.el7.elrepo.x86_64 #1 SMP Fri Dec 21 11:06:36 EST 2018 x86_64 x86_64 x86_64 GNU/Linux
[root@k8s-master01 ~]#

#所有节点安装ipvsadm：
yum install ipvsadm ipset sysstat conntrack libseccomp -y

#所有节点配置ipvs模块，在内核4.19+版本nf_conntrack_ipv4已经改为nf_conntrack， 4.18以下使用nf_conntrack_ipv4即可
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack
echo '
ip_vs
ip_vs_lc
ip_vs_wlc
ip_vs_rr
ip_vs_wrr
ip_vs_lblc
ip_vs_lblcr
ip_vs_dh
ip_vs_sh
ip_vs_fo
ip_vs_nq
ip_vs_sed
ip_vs_ftp
ip_vs_sh
nf_conntrack
ip_tables
ip_set
xt_set
ipt_set
ipt_rpfilter
ipt_REJECT
ipip
' >> /etc/modules-load.d/ipvs.conf 

systemctl enable --now systemd-modules-load.service

#开启一些k8s集群中必须的内核参数，所有节点配置k8s内核
cat <<EOF > /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
fs.may_detach_mounts = 1
net.ipv4.conf.all.route_localnet = 1
vm.overcommit_memory=1
vm.panic_on_oom=0
fs.inotify.max_user_watches=89100
fs.file-max=52706963
fs.nr_open=52706963
net.netfilter.nf_conntrack_max=2310720
net.ipv4.tcp_keepalive_time = 600
net.ipv4.tcp_keepalive_probes = 3
net.ipv4.tcp_keepalive_intvl =15
net.ipv4.tcp_max_tw_buckets = 36000
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_max_orphans = 327680
net.ipv4.tcp_orphan_retries = 3
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.ip_conntrack_max = 65536
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_timestamps = 0
net.core.somaxconn = 16384
EOF

sysctl --system


#所有节点配置完内核后，重启服务器，保证重启后内核依旧加载
reboot

lsmod | grep --color=auto -e ip_vs -e nf_conntrack

```

## 1.3 kubeadm高可用安装k8s集群

如果安装的版本低于1.24，选择Docker和Containerd均可，高于1.24选择Containerd作为Runtime。

### 1.3.1Containerd作为Runtime

```shell
# 所有节点安装docker-ce-20.10
yum install docker-ce-20.10.* docker-ce-cli-20.10.* -y

# 配置Containerd所需的模块（所有节点）
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

# 所有节点加载模块
modprobe -- overlay
modprobe -- br_netfilter

#所有节点，配置Containerd所需的内核
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

sysctl --system

# 所有节点配置Containerd的配置文件
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml

#所有节点将Containerd的Cgroup改为Systemd
vim /etc/containerd/config.toml

#1. 找到containerd.runtimes.runc.options，添加 SystemdCgroup = true
#2. 所有节点将sandbox_image的Pause镜像改成符合自己版本的地址registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.6

#所有节点启动Containerd，并配置开机自启动
systemctl daemon-reload
systemctl enable --now containerd

#所有节点配置crictl客户端连接的运行时位置
cat > /etc/crictl.yaml <<EOF
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 10
debug: false
EOF

```

### 1.3.2 Docker作为Runtime（版本小于1.24）

```shell
#所有节点安装docker-ce 20.10
yum install docker-ce-20.10.* docker-ce-cli-20.10.* -y

#由于新版Kubelet建议使用systemd，所以把Docker的CgroupDriver也改成systemd
mkdir /etc/docker
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"]
}
EOF

#所有节点设置开机自启动Docker
systemctl daemon-reload && systemctl enable --now docker

```

### 1.3.3 安装Kubernetes组件

```shell
#首先在Master01节点查看最新的Kubernetes版本是多少
yum list kubeadm.x86_64 --showduplicates | sort -r

#所有节点安装1.23最新版本kubeadm、kubelet和kubectl：
yum install kubeadm-1.23* kubelet-1.23* kubectl-1.23* -y

# 如果选择的是Containerd作为的Runtime，需要更改Kubelet的配置使用Containerd作为Runtime
cat >/etc/sysconfig/kubelet<<EOF
KUBELET_KUBEADM_ARGS="--container-runtime=remote --runtime-request-timeout=15m --container-runtime-endpoint=unix:///run/containerd/containerd.sock"
EOF


#所有节点设置Kubelet开机自启动
systemctl daemon-reload
systemctl enable --now kubelet

#####################################
#此时kubelet是起不来的，日志会有报错不影响！


```

### 1.3.4 高可用组件安装(环境不够)

### 1.3.5 集群初始化

官方初始化文档：

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/

<font color=red>以下操作只在master01节点执行</font>

+ 配置kubeadm-config.yaml

```yaml
apiVersion: kubeadm.k8s.io/v1beta2
bootstrapTokens:
- groups:
  - system:bootstrappers:kubeadm:default-node-token
  token: 7t2weq.bjbawausm0jaxury
  ttl: 24h0m0s
  usages:
  - signing
  - authentication
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: 172.20.151.134
  bindPort: 6443
nodeRegistration:
  # criSocket: /var/run/dockershim.sock  # 如果是Docker作为Runtime配置此项
  criSocket: /run/containerd/containerd.sock # 如果是Containerd作为Runtime配置此项
  name: k8s-master01
  taints:
  - effect: NoSchedule
    key: node-role.kubernetes.io/master
---
apiServer:
  certSANs:
  - 172.20.151.134
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta2
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controlPlaneEndpoint: 172.20.151.134:6443
controllerManager: {}
dns:
  type: CoreDNS
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers
kind: ClusterConfiguration
kubernetesVersion: v1.23.5 # 更改此处的版本号和kubeadm version一致
networking:
  dnsDomain: cluster.local
  podSubnet: 10.244.0.0/16
  serviceSubnet: 10.96.0.0/16
scheduler: {}

```

+ 更新kubeadm文件

```shell
kubeadm config migrate --old-config kubeadm-config.yaml --new-config new.yaml
```

+ ***\*所有节点\****设置开机自启动kubelet

```shell
systemctl enable --now kubelet #如果启动失败无需管理，初始化成功以后即可启动
```

+ Master01节点初始化

```shell
kubeadm init --config /root/new.yaml  --upload-certs
```

>  初始化以后会在/etc/kubernetes目录下生成对应的证书和配置文件，之后其他Master节点加入Master01

>如果初始化失败，重置后再次初始化，命令如下（没有失败不要执行）：
>
>```shell
>kubeadm reset -f ; ipvsadm --clear  ; rm -rf ~/.kube
>```
>
>

```shell
[root@k8s-master01 ~]# kubeadm init --config /root/new.yaml  --upload-certs
[init] Using Kubernetes version: v1.23.5
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master01 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.20.151.134]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master01 localhost] and IPs [172.20.151.134 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master01 localhost] and IPs [172.20.151.134 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 12.503171 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.23" in namespace kube-system with the configuration for the kubelets in the cluster
NOTE: The "kubelet-config-1.23" naming of the kubelet ConfigMap is deprecated. Once the UnversionedKubeletConfigMap feature gate graduates to Beta the default name will become just "kubelet-config". Kubeadm upgrade will handle this transition transparently.
[upload-certs] Storing the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[upload-certs] Using certificate key:
8eac4f06423dc23963fd06a87448c5298a20a95f27961114b679709ab51653c8
[mark-control-plane] Marking the node k8s-master01 as control-plane by adding the labels: [node-role.kubernetes.io/master(deprecated) node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
[mark-control-plane] Marking the node k8s-master01 as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: 7t2weq.bjbawausm0jaxury
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 172.20.151.134:6443 --token 7t2weq.bjbawausm0jaxury \
        --discovery-token-ca-cert-hash sha256:20d01614dc13baaca8cc15a6fc501cb92a4a1da9b8d4cb47022de3454d807221 \
        --control-plane --certificate-key 8eac4f06423dc23963fd06a87448c5298a20a95f27961114b679709ab51653c8

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.20.151.134:6443 --token 7t2weq.bjbawausm0jaxury \
        --discovery-token-ca-cert-hash sha256:20d01614dc13baaca8cc15a6fc501cb92a4a1da9b8d4cb47022de3454d807221
[root@k8s-master01 ~]#

```

+ Master01节点配置环境变量，用于访问Kubernetes集群

```shell
cat <<EOF >> /root/.bashrc
export KUBECONFIG=/etc/kubernetes/admin.conf
EOF
source /root/.bashrc
```

+ 查看节点状态：

```shell
[root@k8s-master01 ~]# kubectl get node
NAME           STATUS     ROLES                  AGE   VERSION
k8s-master01   NotReady   control-plane,master   12m   v1.23.5
[root@k8s-master01 ~]#
```

+ 查看pod状态

```shell
[root@k8s-master01 ~]# kubectl get pod -n kube-system
NAME                                   READY   STATUS    RESTARTS   AGE
coredns-65c54cc984-mhd8d               0/1     Pending   0          14m
coredns-65c54cc984-mvl8t               0/1     Pending   0          14m
etcd-k8s-master01                      1/1     Running   0          14m
kube-apiserver-k8s-master01            1/1     Running   0          14m
kube-controller-manager-k8s-master01   1/1     Running   0          14m
kube-proxy-vjhn6                       1/1     Running   0          14m
kube-scheduler-k8s-master01            1/1     Running   0          14m
[root@k8s-master01 ~]#
```

> 采用初始化安装方式，所有的系统组件均以容器的方式运行并且在kube-system命名空间内

### 1.3.6 高可用Master(环境不够)

其他master加入集群，master02和master03分别执行(initr操作信息里有)

```shell
 kubeadm join 172.20.151.134:6443 --token 7t2weq.bjbawausm0jaxury \
        --discovery-token-ca-cert-hash sha256:20d01614dc13baaca8cc15a6fc501cb92a4a1da9b8d4cb47022de3454d807221 \
        --control-plane --certificate-key 8eac4f06423dc23963fd06a87448c5298a20a95f27961114b679709ab51653c8
```

查看当前状态:

```shell
[root@k8s-master01 ~]# kubectl get node
NAME           STATUS     ROLES                  AGE   VERSION
k8s-master01   NotReady   control-plane,master   17m   v1.23.5
[root@k8s-master01 ~]#
```

+ Token过期后处理

```shell
#Token过期后生成新的token：
kubeadm token create --print-join-command

#Master需要生成--certificate-key
kubeadm init phase upload-certs  --upload-certs
```







### 1.3.7 Node节点的配置

> Node节点上主要部署公司的一些业务应用，生产环境中不建议Master节点部署系统组件之外的其他Pod，测试环境可以允许Master节点部署Pod以节省系统资源。

+ Node节点加入命令（init信息的最后部分）

```shell
kubeadm join 172.20.151.134:6443 --token 7t2weq.bjbawausm0jaxury \
        --discovery-token-ca-cert-hash sha256:20d01614dc13baaca8cc15a6fc501cb92a4a1da9b8d4cb47022de3454d807221
```

+ k8s-node01上执行

```shell
[root@k8s-node01 ~]# kubeadm join 172.20.151.134:6443 --token 7t2weq.bjbawausm0jaxury \
>         --discovery-token-ca-cert-hash sha256:20d01614dc13baaca8cc15a6fc501cb92a4a1da9b8d4cb47022de3454d807221
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

[root@k8s-node01 ~]#
```

+ k8s-node02上执行

```shell
[root@k8s-node02 ~]# kubeadm join 172.20.151.134:6443 --token 7t2weq.bjbawausm0jaxury \
>         --discovery-token-ca-cert-hash sha256:20d01614dc13baaca8cc15a6fc501cb92a4a1da9b8d4cb47022de3454d807221
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

[root@k8s-node02 ~]#
```

+ 所有节点初始化完成后，查看集群状态

```shell
[root@k8s-master01 ~]# kubectl get node
NAME           STATUS     ROLES                  AGE     VERSION
k8s-master01   NotReady   control-plane,master   27m     v1.23.5
k8s-node01     NotReady   <none>                 4m51s   v1.23.5
k8s-node02     NotReady   <none>                 99s     v1.23.5
[root@k8s-master01 ~]#
```

### 1.3.8 Calico组件的安装

> 用于跨节点通讯，<font color=red>以下操作只在master01节点执行</font>

```shell
git clone https://github.com/dotbalo/k8s-ha-install.git
git branch -a 
git checkout manual-installation-v1.23.x

#cd /root/k8s-ha-install && git checkout manual-installation-v1.23.x && cd calico/
cd /root/study/k8s-ha-install-manual-installation-v1.23.x

#修改Pod网段
POD_SUBNET=`cat /etc/kubernetes/manifests/kube-controller-manager.yaml | grep cluster-cidr= | awk -F= '{print $NF}'`
sed -i "s#POD_CIDR#${POD_SUBNET}#g" calico.yaml
#安装calico
kubectl apply -f calico.yaml

##############################################################################
[root@k8s-master01 calico]# kubectl apply -f calico.yaml
configmap/calico-config created
customresourcedefinition.apiextensions.k8s.io/bgpconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgppeers.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/blockaffinities.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/caliconodestatuses.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/clusterinformations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/felixconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworksets.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/hostendpoints.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamblocks.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamconfigs.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamhandles.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ippools.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipreservations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/kubecontrollersconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networksets.crd.projectcalico.org created
clusterrole.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrolebinding.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrole.rbac.authorization.k8s.io/calico-node created
clusterrolebinding.rbac.authorization.k8s.io/calico-node created
service/calico-typha created
deployment.apps/calico-typha created
Warning: policy/v1beta1 PodDisruptionBudget is deprecated in v1.21+, unavailable in v1.25+; use policy/v1 PodDisruptionBudget
poddisruptionbudget.policy/calico-typha created
daemonset.apps/calico-node created
serviceaccount/calico-node created
deployment.apps/calico-kube-controllers created
serviceaccount/calico-kube-controllers created
poddisruptionbudget.policy/calico-kube-controllers created
[root@k8s-master01 calico]#
[root@k8s-master01 calico]#
[root@k8s-master01 calico]# #查看容器和节点状态
[root@k8s-master01 calico]# kubectl get po -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-6f6595874c-4lvj4   1/1     Running   0          2m20s
calico-node-fgpg6                          1/1     Running   0          2m21s
calico-node-gp5hx                          1/1     Running   0          2m21s
calico-node-h8rls                          1/1     Running   0          2m21s
calico-typha-6b6cf8cbdf-n78pv              1/1     Running   0          2m21s
coredns-65c54cc984-mhd8d                   1/1     Running   0          78m
coredns-65c54cc984-mvl8t                   1/1     Running   0          78m
etcd-k8s-master01                          1/1     Running   0          78m
kube-apiserver-k8s-master01                1/1     Running   0          78m
kube-controller-manager-k8s-master01       1/1     Running   0          78m
kube-proxy-2zlq5                           1/1     Running   0          53m
kube-proxy-494ht                           1/1     Running   0          56m
kube-proxy-vjhn6                           1/1     Running   0          78m
kube-scheduler-k8s-master01                1/1     Running   0          78m
[root@k8s-master01 calico]#
[root@k8s-master01 calico]#
[root@k8s-master01 calico]#
[root@k8s-master01 calico]# kubectl get node
NAME           STATUS   ROLES                  AGE   VERSION
k8s-master01   Ready    control-plane,master   79m   v1.23.5
k8s-node01     Ready    <none>                 57m   v1.23.5
k8s-node02     Ready    <none>                 54m   v1.23.5
[root@k8s-master01 calico]#
[root@k8s-master01 calico]# kubectl get node -owide
NAME           STATUS   ROLES                  AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION                CONTAINER-RUNTIME
k8s-master01   Ready    control-plane,master   80m   v1.23.5   172.20.151.134   <none>        CentOS Linux 7 (Core)   4.19.12-1.el7.elrepo.x86_64   containerd://1.5.11
k8s-node01     Ready    <none>                 58m   v1.23.5   172.20.151.135   <none>        CentOS Linux 7 (Core)   4.19.12-1.el7.elrepo.x86_64   containerd://1.5.11
k8s-node02     Ready    <none>                 54m   v1.23.5   172.20.151.136   <none>        CentOS Linux 7 (Core)   4.19.12-1.el7.elrepo.x86_64   containerd://1.5.11
[root@k8s-master01 calico]#


```

### 1.3.8 Metrics部署

> 在新版的Kubernetes中系统资源的采集均使用Metrics-server，可以通过Metrics采集节点和Pod的内存、磁盘、CPU和网络的使用率。

+ 将Master01节点的front-proxy-ca.crt复制到所有Node节点

```shell
[root@k8s-master01 calico]# scp /etc/kubernetes/pki/front-proxy-ca.crt k8s-node01:/etc/kubernetes/pki/front-proxy-ca.crt
front-proxy-ca.crt                                                       100% 1115     2.9MB/s   00:00
[root@k8s-master01 calico]# scp /etc/kubernetes/pki/front-proxy-ca.crt k8s-node02:/etc/kubernetes/pki/front-proxy-ca.crt
front-proxy-ca.crt                                                       100% 1115     3.3MB/s   00:00
[root@k8s-master01 calico]#

```

+ 安装metrics server

```shell
#cd /root/k8s-ha-install/kubeadm-metrics-server

[root@k8s-master01 kubeadm-metrics-server]# pwd
/root/study/k8s-ha-install-manual-installation-v1.23.x/kubeadm-metrics-server
[root@k8s-master01 kubeadm-metrics-server]# ls
comp.yaml
[root@k8s-master01 kubeadm-metrics-server]# kubectl  create -f comp.yaml
serviceaccount/metrics-server created
clusterrole.rbac.authorization.k8s.io/system:aggregated-metrics-reader created
clusterrole.rbac.authorization.k8s.io/system:metrics-server created
rolebinding.rbac.authorization.k8s.io/metrics-server-auth-reader created
clusterrolebinding.rbac.authorization.k8s.io/metrics-server:system:auth-delegator created
clusterrolebinding.rbac.authorization.k8s.io/system:metrics-server created
service/metrics-server created
deployment.apps/metrics-server created
apiservice.apiregistration.k8s.io/v1beta1.metrics.k8s.io created
[root@k8s-master01 kubeadm-metrics-server]#

```

+ 查看metrics-server状态

```shell
[root@k8s-master01 kubeadm-metrics-server]# kubectl get po -n kube-system -l k8s-app=metrics-server
NAME                              READY   STATUS    RESTARTS   AGE
metrics-server-5cf8885b66-p8r9h   1/1     Running   0          41s
[root@k8s-master01 kubeadm-metrics-server]#

```

+ 查看node/pod的top状态

```shell
[root@k8s-master01 kubeadm-metrics-server]# kubectl top node
NAME           CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
k8s-master01   123m         6%     1473Mi          38%
k8s-node01     79m          3%     975Mi           25%
k8s-node02     58m          2%     906Mi           23%
[root@k8s-master01 kubeadm-metrics-server]#
[root@k8s-master01 kubeadm-metrics-server]#
[root@k8s-master01 kubeadm-metrics-server]#
[root@k8s-master01 kubeadm-metrics-server]# kubectl top po -A
NAMESPACE     NAME                                       CPU(cores)   MEMORY(bytes)
kube-system   calico-kube-controllers-6f6595874c-4lvj4   3m           21Mi
kube-system   calico-node-fgpg6                          22m          132Mi
kube-system   calico-node-gp5hx                          38m          111Mi
kube-system   calico-node-h8rls                          23m          127Mi
kube-system   calico-typha-6b6cf8cbdf-n78pv              2m           27Mi
kube-system   coredns-65c54cc984-mhd8d                   1m           17Mi
kube-system   coredns-65c54cc984-mvl8t                   1m           16Mi
kube-system   etcd-k8s-master01                          12m          52Mi
kube-system   kube-apiserver-k8s-master01                38m          308Mi
kube-system   kube-controller-manager-k8s-master01       10m          55Mi
kube-system   kube-proxy-2zlq5                           1m           17Mi
kube-system   kube-proxy-494ht                           1m           17Mi
kube-system   kube-proxy-vjhn6                           1m           16Mi
kube-system   kube-scheduler-k8s-master01                4m           23Mi
kube-system   metrics-server-5cf8885b66-p8r9h            2m           13Mi
[root@k8s-master01 kubeadm-metrics-server]#

```

### 1.3.10 Dashboard部署

> Dashboard用于展示集群中的各类资源，同时也可以通过Dashboard实时查看Pod的日志和在容器中执行一些命令等。

#### 1.3.10.1 安装指定版本dashboard

```shell
[root@k8s-master01 k8s-ha-install-manual-installation-v1.23.x]# cd dashboard/
[root@k8s-master01 dashboard]# ls
dashboard-user.yaml  dashboard.yaml
[root@k8s-master01 dashboard]# pwd
/root/study/k8s-ha-install-manual-installation-v1.23.x/dashboard
[root@k8s-master01 dashboard]#
[root@k8s-master01 dashboard]#
[root@k8s-master01 dashboard]# ls
dashboard-user.yaml  dashboard.yaml
[root@k8s-master01 dashboard]#
[root@k8s-master01 dashboard]#
[root@k8s-master01 dashboard]# kubectl  create -f .    #执行安装命令
serviceaccount/admin-user created
clusterrolebinding.rbac.authorization.k8s.io/admin-user created
namespace/kubernetes-dashboard created
serviceaccount/kubernetes-dashboard created
service/kubernetes-dashboard created
secret/kubernetes-dashboard-certs created
secret/kubernetes-dashboard-csrf created
secret/kubernetes-dashboard-key-holder created
configmap/kubernetes-dashboard-settings created
role.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
deployment.apps/kubernetes-dashboard created
service/dashboard-metrics-scraper created
deployment.apps/dashboard-metrics-scraper created
[root@k8s-master01 dashboard]#

```

#### 1.3.10.2 安装最新版(未做)

官方GitHub地址：https://github.com/kubernetes/dashboard 可以在官方dashboard查看到最新版dashboard，类似 ：

> kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml

2.0.3 以具体版本号为准

```shell
vim admin.yaml

apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding 
metadata: 
  name: admin-user
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
```

kubectl apply -f admin.yaml -n kube-system

#### 1.3.10.3 登录dashboard

在谷歌浏览器（Chrome）启动文件中加入启动参数，用于解决无法访问Dashboard的问题，` --test-type --ignore-certificate-errors `

+ 更改dashboard的svc为NodePort：

```shell
kubectl edit svc kubernetes-dashboard -n kubernetes-dashboard
# 看 type: NodePort ，即可；如果不是，则改为此。
```

+ 查看端口号

```shell
[root@k8s-master01 dashboard]# kubectl get svc kubernetes-dashboard -n kubernetes-dashboard
NAME                   TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
kubernetes-dashboard   NodePort   10.96.152.252   <none>        443:31591/TCP   13h
[root@k8s-master01 dashboard]#
[root@k8s-master01 dashboard]# netstat -npa | grep 31591
tcp        0      0 0.0.0.0:31591           0.0.0.0:*               LISTEN      16717/kube-proxy
tcp        0      0 172.20.151.134:45866    172.20.151.134:31591    ESTABLISHED 27250/sshd: root
[root@k8s-master01 dashboard]#
```

+ 访问Dashboard，填入token即可访问

<https://172.20.151.134:31591/#/login>

+ 获取token的方法

```shell
[root@k8s-master01 dashboard]# kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')
Name:         admin-user-token-l7vw5
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: admin-user
              kubernetes.io/service-account.uid: 60291cd8-1521-4422-bd5a-fba8e2c856db

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1099 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6Ik8xMEg3Y3NJeV9pd1lROGp6ZE14QTRJYlM1Q1hyQ0s5V0xlclo3WDhyc0EifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLWw3dnc1Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiI2MDI5MWNkOC0xNTIxLTQ0MjItYmQ1YS1mYmE4ZTJjODU2ZGIiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06YWRtaW4tdXNlciJ9.YpCPtYf-qrbPF69NsuF37qS8M34JpqsEGqdeU1agqVgxkN2erPi1hSuAafbTe6dK3zYLzo0FcabYngpC6WPthMFKm6w5PGJgfm6tTswSWC3kqAuoTITfpNzdqwz1xfmCnkfDOLPuH4R6wbVVWuIvfzVgtHwZ_LEbssQajOx7NWp-yyvEplO4PswH4BiV4iieFHxBiLBwALBO4cip6m9iMbuWVOl8vC3I51mC9VvYHFm0ZRWuVGoPkkuIe1h2hzjboOYG8jmYutUxICf6MdA-p6H2yOAkj6xwSuuI8d7haxP6ibW9eNJnu8ME4xbY1u2m3YdY0tmaXToQ33TCY9ngYQ
[root@k8s-master01 dashboard]#
```









 





# 附件

## 附件1 git命令大全

```shell
# Git全局设置（设置一次即可）
git config --global user.name "zhu"
git config --global user.email "zhu@admin.com"

# 推送现有文件夹
cd existing_folder
git init
git remote add origin git@61.155.203.87:zhu/fhops.git
git add .
git commit -m "Initial commit"
git push -u origin master

# 创建一个新仓库（未测试）
git clone git@61.155.203.87:zhu/fhops.git
cd fhops
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

# 推送现有的Git仓库（未测试）
cd existing_repo
git remote rename origin old-origin
git remote add origin git@61.155.203.87:zhu/fhops.git
git push -u origin --all
git push -u origin --tags

*********** 实战 ***********
# 拉取整个项目（首次初始化使用）
git clone git@61.155.203.87:zhu/fhops.git

# 查看分支
git branch                  # 查看本地分支
git branch -a               # 查看所有分支

git checkout -b x0854       # 创建分支（x0854）
git status                  # 查看状态

# 推送新分支
git add .                   # 保存本地修改
git commit -m "xxxx"        # 添加描述信息
git push -u origin x0854    # 推送分支名称（x0854）

# 从master获取
git fetch                   # 获取更新
git merge origin master     # 获取最新master数据

# 删除分支
git checkout master         # 切换到master分支
git branch -d x0854         # 删除本地分支
git push origin -d x0854    # 删除远程分支

*********** 实战20220402 ***********
# 带token摘取git内容，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为ydsl01的token，有效期到20230402
git clone https://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@github.com/ydsl01/kubernetes-study-documents-51.git

```

## 附件2 kubectl常用命令

```shell
# 查看所有 pod 列表,  -n 后跟 namespace, 查看指定的命名空间
kubectl get pod
kubectl get pod -n kube-system  #查看指定命令空间的Pod  
kubectl get pod -o wide		# 查看更详细的信息，比如Pod所在的节点
kubectl get pod --show-labels  #获取pod并查看pod的标签
kubectl get pod -A	#查看所有pod

# 查看 RC 和 service 列表， -o wide 查看详细信息
kubectl get rc,svc
kubectl get pod,svc -o wide  
kubectl get pod <pod-name> -o yaml


# 显示 Node 的详细信息
kubectl describe node 192.168.0.212 #可以跟Node IP或者主机名


# 显示 Pod 的详细信息, 特别是查看 pod 无法创建的时候的日志
kubectl describe pod <pod-name>
eg:
kubectl describe pod redis-master-tqds9


# 根据 yaml 创建资源, apply 可以重复执行，create 不行
kubectl create -f pod.yaml
kubectl apply -f pod.yaml


# 基于 pod.yaml 定义的名称删除指定资源
kubectl delete -f pod.yaml 


# 删除所有包含某个 label 的pod 和 service
kubectl delete pod,svc -l name=<label-name>


# 删除默认命名空间下的所有 Pod
kubectl delete pod --all


# 执行 pod 命令
kubectl exec <pod-name> -- date
kubectl exec <pod-name> -- bash
kubectl exec <pod-name> -- ping 10.24.51.9


# 通过bash获得 pod 中某个容器的TTY，相当于登录容器
kubectl exec -it <pod-name> -c <container-name> -- bash
eg:
kubectl exec -it redis-master-cln81 -- bash


# 查看容器的日志
kubectl logs <pod-name>
kubectl logs -f <pod-name> # 实时查看日志
kubectl log  <pod-name>  -c <container_name> # 若 pod 只有一个容器，可以不加 -c 

kubectl logs -l app=frontend # 返回所有标记为 app=frontend 的 pod 的合并日志。


# 查看节点 labels
kubectl get node --show-labels

# 重启 pod
kubectl get pod <POD名称> -n <NAMESPACE名称> -o yaml | kubectl replace --force -f -



```





### 附2.1 命令自动补全

```shell
source <(kubectl completion bash) # 在 bash 中设置当前 shell 的自动补全，要先安装 bash-completion 包。

echo "source <(kubectl completion bash)" >> ~/.bashrc 
```


###  附2.2 创建命令

```shell
kubectl apply -f ./my-manifest.yaml           # 创建资源
kubectl apply -f ./my1.yaml -f ./my2.yaml     # 使用多个文件创建
kubectl apply -f ./dir                        # 基于目录下的所有清单文件创建资源
kubectl apply -f https://git.io/vPieo         # 从 URL 中创建资源
kubectl create deployment nginx --image=nginx # 启动单实例 nginx
kubectl explain pods,svc                      # 获取 pod 清单的文档说明

# 从标准输入创建多个 YAML 对象
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "1000000"
---
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep-less
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "1000"
EOF

# 创建有多个 key 的 Secret
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  password: $(echo -n "s33msi4" | base64 -w0)
  username: $(echo -n "jane" | base64 -w0)
EOF

```

### 附2.3 查看和查找资源

```shell
# get 命令的基本输出
kubectl get services                          # 列出当前命名空间下的所有 services
kubectl get pods --all-namespaces             # 列出所有命名空间下的全部的 Pods
kubectl get pods -o wide                      # 列出当前命名空间下的全部 Pods，并显示更详细的信息
kubectl get deployment my-dep                 # 列出某个特定的 Deployment
kubectl get pods                              # 列出当前命名空间下的全部 Pods
kubectl get pod my-pod -o yaml                # 获取一个 pod 的 YAML

# describe 命令的详细输出
kubectl describe nodes my-node
kubectl describe pods my-pod

# 列出当前名字空间下所有 Services，按名称排序
kubectl get services --sort-by=.metadata.name

# 列出 Pods，按重启次数排序
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'

# 列举所有 PV 持久卷，按容量排序
kubectl get pv --sort-by=.spec.capacity.storage

# 获取包含 app=cassandra 标签的所有 Pods 的 version 标签
kubectl get pods --selector=app=cassandra -o \
  jsonpath='{.items[*].metadata.labels.version}'

# 获取所有工作节点（使用选择器以排除标签名称为 'node-role.kubernetes.io/master' 的结果）
kubectl get node --selector='!node-role.kubernetes.io/master'

# 获取当前命名空间中正在运行的 Pods
kubectl get pods --field-selector=status.phase=Running

# 获取全部节点的 ExternalIP 地址
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'

# 列出属于某个特定 RC 的 Pods 的名称
# 在转换对于 jsonpath 过于复杂的场合，"jq" 命令很有用；可以在 https://stedolan.github.io/jq/ 找到它。
sel=${$(kubectl get rc my-rc --output=json | jq -j '.spec.selector | to_entries | .[] | "\(.key)=\(.value),"')%?}
echo $(kubectl get pods --selector=$sel --output=jsonpath={.items..metadata.name})

# 显示所有 Pods 的标签（或任何其他支持标签的 Kubernetes 对象）
kubectl get pods --show-labels

# 检查哪些节点处于就绪状态
JSONPATH='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}' \
 && kubectl get nodes -o jsonpath="$JSONPATH" | grep "Ready=True"

# 列出被一个 Pod 使用的全部 Secret
kubectl get pods -o json | jq '.items[].spec.containers[].env[]?.valueFrom.secretKeyRef.name' | grep -v null | sort | uniq

# 列举所有 Pods 中初始化容器的容器 ID（containerID）
# Helpful when cleaning up stopped containers, while avoiding removal of initContainers.
kubectl get pods --all-namespaces -o jsonpath='{range .items[*].status.initContainerStatuses[*]}{.containerID}{"\n"}{end}' | cut -d/ -f3

# 列出事件（Events），按时间戳排序
kubectl get events --sort-by=.metadata.creationTimestamp

# 比较当前的集群状态和假定某清单被应用之后的集群状态
kubectl diff -f ./my-manifest.yaml
```

 

### 附2.4 更新资源

```shell
kubectl set image deployment/frontend www=image:v2               # 滚动更新 "frontend" Deployment 的 "www" 容器镜像
kubectl rollout history deployment/frontend                      # 检查 Deployment 的历史记录，包括版本 
kubectl rollout undo deployment/frontend                         # 回滚到上次部署版本
kubectl rollout undo deployment/frontend --to-revision=2         # 回滚到特定部署版本
kubectl rollout status -w deployment/frontend                    # 监视 "frontend" Deployment 的滚动升级状态直到完成
kubectl rollout restart deployment/frontend                      # 轮替重启 "frontend" Deployment

cat pod.json | kubectl replace -f -                              # 通过传入到标准输入的 JSON 来替换 Pod

# 强制替换，删除后重建资源。会导致服务不可用。
kubectl replace --force -f ./pod.json

# 为多副本的 nginx 创建服务，使用 80 端口提供服务，连接到容器的 8000 端口。
kubectl expose rc nginx --port=80 --target-port=8000

# 将某单容器 Pod 的镜像版本（标签）更新到 v4
kubectl get pod mypod -o yaml | sed 's/\(image: myimage\):.*$/\1:v4/' | kubectl replace -f -

kubectl label pods my-pod new-label=awesome                      # 添加标签
kubectl annotate pods my-pod icon-url=http://goo.gl/XXBTWq       # 添加注解
kubectl autoscale deployment foo --min=2 --max=10                # 对 "foo" Deployment 自动伸缩容
```

### 附2.5 部分更新资源

```shell
# 部分更新某节点
kubectl patch node k8s-node-1 -p '{"spec":{"unschedulable":true}}' 

# 更新容器的镜像；spec.containers[*].name 是必须的。因为它是一个合并性质的主键。
kubectl patch pod valid-pod -p '{"spec":{"containers":[{"name":"kubernetes-serve-hostname","image":"new image"}]}}'

# 使用带位置数组的 JSON patch 更新容器的镜像
kubectl patch pod valid-pod --type='json' -p='[{"op": "replace", "path": "/spec/containers/0/image", "value":"new image"}]'

# 使用带位置数组的 JSON patch 禁用某 Deployment 的 livenessProbe
kubectl patch deployment valid-deployment  --type json   -p='[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'

# 在带位置数组中添加元素 
kubectl patch sa default --type='json' -p='[{"op": "add", "path": "/secrets/1", "value": {"name": "whatever" } }]'
```

 

### 附2.6 删除资源

```shell
kubectl delete -f ./pod.json                                              # 删除在 pod.json 中指定的类型和名称的 Pod
kubectl delete pod,service baz foo                                        # 删除名称为 "baz" 和 "foo" 的 Pod 和服务
kubectl delete pods,services -l name=myLabel                              # 删除包含 name=myLabel 标签的 pods 和服务
kubectl delete pods,services -l name=myLabel --include-uninitialized      # 删除包含 label name=myLabel 标签的 Pods 和服务
kubectl -n my-ns delete po,svc --all                                      # 删除在 my-ns 名字空间中全部的 Pods 和服务
# 删除所有与 pattern1 或 pattern2 awk 模式匹配的 Pods
kubectl get pods  -n mynamespace --no-headers=true | awk '/pattern1|pattern2/{print $1}' | xargs  kubectl delete -n mynamespace pod
```


### 附2.7 Pod常用操作

```shell
kubectl logs my-pod                                 # 获取 pod 日志（标准输出）
kubectl logs -l name=myLabel                        # 获取含 name=myLabel 标签的 Pods 的日志（标准输出）
kubectl logs my-pod --previous                      # 获取上个容器实例的 pod 日志（标准输出）
kubectl logs my-pod -c my-container                 # 获取 Pod 容器的日志（标准输出, 多容器场景）
kubectl logs -l name=myLabel -c my-container        # 获取含 name=myLabel 标签的 Pod 容器日志（标准输出, 多容器场景）
kubectl logs my-pod -c my-container --previous      # 获取 Pod 中某容器的上个实例的日志（标准输出, 多容器场景）
kubectl logs -f my-pod                              # 流式输出 Pod 的日志（标准输出）
kubectl logs -f my-pod -c my-container              # 流式输出 Pod 容器的日志（标准输出, 多容器场景）
kubectl logs -f -l name=myLabel --all-containers    # 流式输出含 name=myLabel 标签的 Pod 的所有日志（标准输出）
kubectl run -i --tty busybox --image=busybox -- sh  # 以交互式 Shell 运行 Pod
kubectl run nginx --image=nginx -n mynamespace      # 在指定名字空间中运行 nginx Pod
kubectl run nginx --image=nginx                     # 运行 ngins Pod 并将其规约写入到名为 pod.yaml 的文件
  --dry-run=client -o yaml > pod.yaml

kubectl attach my-pod -i                            # 挂接到一个运行的容器中
kubectl port-forward my-pod 5000:6000               # 在本地计算机上侦听端口 5000 并转发到 my-pod 上的端口 6000
kubectl exec my-pod -- ls /                         # 在已有的 Pod 中运行命令（单容器场景）
kubectl exec my-pod -c my-container -- ls /         # 在已有的 Pod 中运行命令（多容器场景）
kubectl top pod POD_NAME --containers               # 显示给定 Pod 和其中容器的监控数据
```

### 附2.8 节点操作

```shell
kubectl cordon my-node                                                # 标记 my-node 节点为不可调度
kubectl drain my-node                                                 # 对 my-node 节点进行清空操作，为节点维护做准备
kubectl uncordon my-node                                              # 标记 my-node 节点为可以调度
kubectl top node my-node                                              # 显示给定节点的度量值
kubectl cluster-info                                                  # 显示主控节点和服务的地址
kubectl cluster-info dump                                             # 将当前集群状态转储到标准输出
kubectl cluster-info dump --output-directory=/path/to/cluster-state   # 将当前集群状态输出到 /path/to/cluster-state

# 如果已存在具有指定键和效果的污点，则替换其值为指定值
kubectl taint nodes foo dedicated=special-user:NoSchedule
```



### 附2.9 格式化输出

要以特定格式将详细信息输出到终端窗口，可以将 -o 或 --output 参数添加到支持的 kubectl 命令。

| 输出格式                          | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| -o=custom-columns=<spec>          | 使用逗号分隔的自定义列来打印表格                             |
| -o=custom-columns-file=<filename> | 使用 <filename> 文件中的自定义列模板打印表格                 |
| -o=json                           | 输出 JSON 格式的 API 对象                                    |
| -o=jsonpath=<template>            | 打印 [jsonpath](https://kubernetes.io/docs/reference/kubectl/jsonpath) 表达式中定义的字段 |
| -o=jsonpath-file=<filename>       | 打印在 <filename> 文件中定义的 [jsonpath](https://kubernetes.io/docs/reference/kubectl/jsonpath) 表达式所指定的字段。 |
| -o=name                           | 仅打印资源名称而不打印其他内容                               |
| -o=wide                           | 以纯文本格式输出额外信息，对于 Pod 来说，输出中包含了节点名称 |
| -o=yaml                           | 输出 YAML 格式的 API 对象                                    |

使用 -o=custom-columns 的示例：

```shell
# 集群中运行着的所有镜像
kubectl get pods -A -o=custom-columns='DATA:spec.containers[*].image'

 # 除 "k8s.gcr.io/coredns:1.6.2" 之外的所有镜像
kubectl get pods -A -o=custom-columns='DATA:spec.containers[?(@.image!="k8s.gcr.io/coredns:1.6.2")].image'

# 输出 metadata 下面的所有字段，无论 Pod 名字为何
kubectl get pods -A -o=custom-columns='DATA:metadata.*'

```





# END

