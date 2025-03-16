
# kubeadm-init-log

k8s init 성공시 볼 수 있는 로그 정리

<br>

```
[root@k8s-master-2 ~]# sudo kubeadm init --image-repository=10.0.16.62:8080/new --control-plane-endpoint=10.0.16.71:6443 --upload-certs --v=5
## 1. CRI 소켓 탐색 -- kubeadm이 컨테이너 런타임 인터페이스 CRI를 감지함 
I0316 08:09:18.502656  737621 initconfiguration.go:122] detected and using CRI socket: unix:///var/run/containerd/containerd.sock
I0316 08:09:18.503554  737621 interface.go:432] Looking for default routes with IPv4 addresses
I0316 08:09:18.503679  737621 interface.go:437] Default route transits interface "eth0"
I0316 08:09:18.503969  737621 interface.go:209] Interface eth0 is up
I0316 08:09:18.504218  737621 interface.go:257] Interface "eth0" has 2 addresses :[10.0.16.71/24 fe80::f816:3eff:fefc:5459/64].
I0316 08:09:18.504371  737621 interface.go:224] Checking addr  10.0.16.71/24.
I0316 08:09:18.504456  737621 interface.go:231] IP found 10.0.16.71

## 2. 네트워크 인터페이스 및 노드 IP 확인
I0316 08:09:18.504543  737621 interface.go:263] Found valid IPv4 address 10.0.16.71 for interface "eth0".
I0316 08:09:18.504662  737621 interface.go:443] Found active IP 10.0.16.71

## 3. Kubelet 설정 확인
I0316 08:09:18.504969  737621 kubelet.go:196] the value of KubeletConfiguration.cgroupDriver is empty; setting it to "systemd"
I0316 08:09:18.528738  737621 version.go:187] fetching Kubernetes version from URL: https://dl.k8s.io/release/stable-1.txt
W0316 08:09:18.552813  737621 version.go:104] could not fetch a Kubernetes version from the internet: unable to get URL "https://dl.k8s.io/release/stable-1.txt": Get "https://dl.k8s.io/release/stable-1.txt": dial tcp: lookup dl.k8s.io on 10.0.16.2:53: server misbehaving
W0316 08:09:18.553123  737621 version.go:105] falling back to the local client version: v1.29.11
[init] Using Kubernetes version: v1.29.11
[preflight] Running pre-flight checks

## 4. 사전 점검 (Pre-flight Checks) - Kubernetes 클러스터를 정상적으로 초기화할 수 있는지 사전 점검 수행
I0316 08:09:18.554538  737621 checks.go:563] validating Kubernetes and kubeadm version
I0316 08:09:18.554831  737621 checks.go:168] validating if the firewall is enabled and active
I0316 08:09:18.602915  737621 checks.go:203] validating availability of port 6443
I0316 08:09:18.603716  737621 checks.go:203] validating availability of port 10259
I0316 08:09:18.603816  737621 checks.go:203] validating availability of port 10257
I0316 08:09:18.604011  737621 checks.go:280] validating the existence of file /etc/kubernetes/manifests/kube-apiserver.yaml
I0316 08:09:18.604063  737621 checks.go:280] validating the existence of file /etc/kubernetes/manifests/kube-controller-manager.yaml
I0316 08:09:18.604107  737621 checks.go:280] validating the existence of file /etc/kubernetes/manifests/kube-scheduler.yaml
I0316 08:09:18.604164  737621 checks.go:280] validating the existence of file /etc/kubernetes/manifests/etcd.yaml
I0316 08:09:18.604300  737621 checks.go:430] validating if the connectivity type is via proxy or direct
I0316 08:09:18.604429  737621 checks.go:469] validating http connectivity to first IP address in the CIDR
I0316 08:09:18.604475  737621 checks.go:469] validating http connectivity to first IP address in the CIDR
I0316 08:09:18.604509  737621 checks.go:104] validating the container runtime
I0316 08:09:18.730215  737621 checks.go:639] validating whether swap is enabled or not
I0316 08:09:18.730521  737621 checks.go:370] validating the presence of executable crictl
I0316 08:09:18.730601  737621 checks.go:370] validating the presence of executable conntrack
I0316 08:09:18.730654  737621 checks.go:370] validating the presence of executable ip
I0316 08:09:18.730701  737621 checks.go:370] validating the presence of executable iptables
I0316 08:09:18.730762  737621 checks.go:370] validating the presence of executable mount
I0316 08:09:18.730838  737621 checks.go:370] validating the presence of executable nsenter
I0316 08:09:18.730906  737621 checks.go:370] validating the presence of executable ethtool
I0316 08:09:18.730939  737621 checks.go:370] validating the presence of executable tc
I0316 08:09:18.730997  737621 checks.go:370] validating the presence of executable touch
I0316 08:09:18.731039  737621 checks.go:516] running all checks
I0316 08:09:18.761905  737621 checks.go:401] checking whether the given node name is valid and reachable using net.LookupHost
I0316 08:09:18.762099  737621 checks.go:605] validating kubelet version
I0316 08:09:18.902113  737621 checks.go:130] validating if the "kubelet" service is enabled and active
	[WARNING Service-Kubelet]: kubelet service is not enabled, please run 'systemctl enable kubelet.service'
I0316 08:09:18.962511  737621 checks.go:203] validating availability of port 10250
I0316 08:09:18.962922  737621 checks.go:329] validating the contents of file /proc/sys/net/bridge/bridge-nf-call-iptables
I0316 08:09:18.963310  737621 checks.go:329] validating the contents of file /proc/sys/net/ipv4/ip_forward
I0316 08:09:18.963381  737621 checks.go:203] validating availability of port 2379
I0316 08:09:18.963512  737621 checks.go:203] validating availability of port 2380
I0316 08:09:18.963706  737621 checks.go:243] validating the existence and emptiness of directory /var/lib/etcd
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'

## 5. 이미지 다운로드 확인
I0316 08:09:18.964216  737621 checks.go:828] using image pull policy: IfNotPresent
I0316 08:09:19.023967  737621 checks.go:846] image exists: 10.0.16.62:8080/new/kube-apiserver:v1.29.11
I0316 08:09:19.067897  737621 checks.go:846] image exists: 10.0.16.62:8080/new/kube-controller-manager:v1.29.11
I0316 08:09:19.121899  737621 checks.go:846] image exists: 10.0.16.62:8080/new/kube-scheduler:v1.29.11
I0316 08:09:19.166452  737621 checks.go:846] image exists: 10.0.16.62:8080/new/kube-proxy:v1.29.11
I0316 08:09:19.227060  737621 checks.go:846] image exists: 10.0.16.62:8080/new/coredns:v1.11.1
I0316 08:09:19.346328  737621 checks.go:846] image exists: 10.0.16.62:8080/new/pause:3.9
I0316 08:09:19.404943  737621 checks.go:846] image exists: 10.0.16.62:8080/new/etcd:3.5.16-0

## 6. 인증서 생성
[certs] Using certificateDir folder "/etc/kubernetes/pki"
I0316 08:09:19.405351  737621 certs.go:112] creating a new certificate authority for ca
[certs] Generating "ca" certificate and key
I0316 08:09:19.705783  737621 certs.go:519] validating certificate period for ca certificate
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master-2 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.0.16.71]
[certs] Generating "apiserver-kubelet-client" certificate and key
I0316 08:09:20.334959  737621 certs.go:112] creating a new certificate authority for front-proxy-ca
[certs] Generating "front-proxy-ca" certificate and key
I0316 08:09:20.612167  737621 certs.go:519] validating certificate period for front-proxy-ca certificate
[certs] Generating "front-proxy-client" certificate and key
I0316 08:09:20.901589  737621 certs.go:112] creating a new certificate authority for etcd-ca
[certs] Generating "etcd/ca" certificate and key
I0316 08:09:21.065271  737621 certs.go:519] validating certificate period for etcd/ca certificate
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master-2 localhost] and IPs [10.0.16.71 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master-2 localhost] and IPs [10.0.16.71 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
I0316 08:09:22.239465  737621 certs.go:78] creating new public/private key files for signing service account users
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
I0316 08:09:22.372382  737621 kubeconfig.go:112] creating kubeconfig file for admin.conf

## 7. Kubeconfig 파일 생성
[kubeconfig] Writing "admin.conf" kubeconfig file
I0316 08:09:22.496756  737621 kubeconfig.go:112] creating kubeconfig file for super-admin.conf
[kubeconfig] Writing "super-admin.conf" kubeconfig file
I0316 08:09:22.792774  737621 kubeconfig.go:112] creating kubeconfig file for kubelet.conf
[kubeconfig] Writing "kubelet.conf" kubeconfig file
I0316 08:09:22.902729  737621 kubeconfig.go:112] creating kubeconfig file for controller-manager.conf
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
I0316 08:09:23.377538  737621 kubeconfig.go:112] creating kubeconfig file for scheduler.conf
[kubeconfig] Writing "scheduler.conf" kubeconfig file

## 8. Etcd 및 Control Plane 구성
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
I0316 08:09:23.577173  737621 local.go:65] [etcd] wrote Static Pod manifest for a local etcd member to "/etc/kubernetes/manifests/etcd.yaml"
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
I0316 08:09:23.577465  737621 manifests.go:102] [control-plane] getting StaticPodSpecs
I0316 08:09:23.577920  737621 certs.go:519] validating certificate period for CA certificate
I0316 08:09:23.578028  737621 manifests.go:128] [control-plane] adding volume "ca-certs" for component "kube-apiserver"
I0316 08:09:23.578045  737621 manifests.go:128] [control-plane] adding volume "etc-pki" for component "kube-apiserver"
I0316 08:09:23.578057  737621 manifests.go:128] [control-plane] adding volume "k8s-certs" for component "kube-apiserver"
I0316 08:09:23.578069  737621 manifests.go:128] [control-plane] adding volume "usr-share-ca-certificates" for component "kube-apiserver"
I0316 08:09:23.579347  737621 manifests.go:157] [control-plane] wrote static Pod manifest for component "kube-apiserver" to "/etc/kubernetes/manifests/kube-apiserver.yaml"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
I0316 08:09:23.579416  737621 manifests.go:102] [control-plane] getting StaticPodSpecs
I0316 08:09:23.579692  737621 manifests.go:128] [control-plane] adding volume "ca-certs" for component "kube-controller-manager"
I0316 08:09:23.579728  737621 manifests.go:128] [control-plane] adding volume "etc-pki" for component "kube-controller-manager"
I0316 08:09:23.579741  737621 manifests.go:128] [control-plane] adding volume "flexvolume-dir" for component "kube-controller-manager"
I0316 08:09:23.579753  737621 manifests.go:128] [control-plane] adding volume "k8s-certs" for component "kube-controller-manager"
I0316 08:09:23.579764  737621 manifests.go:128] [control-plane] adding volume "kubeconfig" for component "kube-controller-manager"
I0316 08:09:23.579779  737621 manifests.go:128] [control-plane] adding volume "usr-share-ca-certificates" for component "kube-controller-manager"
I0316 08:09:23.581881  737621 manifests.go:157] [control-plane] wrote static Pod manifest for component "kube-controller-manager" to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
[control-plane] Creating static Pod manifest for "kube-scheduler"
I0316 08:09:23.581936  737621 manifests.go:102] [control-plane] getting StaticPodSpecs
I0316 08:09:23.582247  737621 manifests.go:128] [control-plane] adding volume "kubeconfig" for component "kube-scheduler"
I0316 08:09:23.583044  737621 manifests.go:157] [control-plane] wrote static Pod manifest for component "kube-scheduler" to "/etc/kubernetes/manifests/kube-scheduler.yaml"
I0316 08:09:23.583145  737621 kubelet.go:68] Stopping the kubelet
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
I0316 08:09:24.001747  737621 waitcontrolplane.go:83] [wait-control-plane] Waiting for the API server to be healthy

## 9. kubeadm init을 실행 -- 에러가 날 경우: kubelet이 static Pod를 실행하지 못하고 있을 가능성이 있음.
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 10.004837 seconds
I0316 08:09:34.015390  737621 kubeconfig.go:606] ensuring that the ClusterRoleBinding for the kubeadm:cluster-admins Group exists
I0316 08:09:34.020403  737621 kubeconfig.go:682] creating the ClusterRoleBinding for the kubeadm:cluster-admins Group by using super-admin.conf
I0316 08:09:34.058422  737621 uploadconfig.go:112] [upload-config] Uploading the kubeadm ClusterConfiguration to a ConfigMap
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
I0316 08:09:34.107281  737621 uploadconfig.go:126] [upload-config] Uploading the kubelet component config to a ConfigMap
[kubelet] Creating a ConfigMap "kubelet-config" in namespace kube-system with the configuration for the kubelets in the cluster
I0316 08:09:34.188089  737621 uploadconfig.go:131] [upload-config] Preserving the CRISocket information for the control-plane node
I0316 08:09:34.188347  737621 patchnode.go:31] [patchnode] Uploading the CRI Socket information "unix:///var/run/containerd/containerd.sock" to the Node API object "k8s-master-2" as an annotation
[upload-certs] Storing the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[upload-certs] Using certificate key:
23911e0dbad42ca9c82419f332ffc9367fa1b3ca5c5ab74011026f96240b4e12
[mark-control-plane] Marking the node k8s-master-2 as control-plane by adding the labels: [node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
[mark-control-plane] Marking the node k8s-master-2 as control-plane by adding the taints [node-role.kubernetes.io/control-plane:NoSchedule]
[bootstrap-token] Using token: sewekg.e85zilfzi0v3vxf3
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] Configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
I0316 08:09:35.526354  737621 clusterinfo.go:47] [bootstrap-token] loading admin kubeconfig
I0316 08:09:35.527022  737621 clusterinfo.go:58] [bootstrap-token] copying the cluster from admin.conf to the bootstrap kubeconfig
I0316 08:09:35.527270  737621 clusterinfo.go:70] [bootstrap-token] creating/updating ConfigMap in kube-public namespace
I0316 08:09:35.538323  737621 clusterinfo.go:84] creating the RBAC rules for exposing the cluster-info ConfigMap in the kube-public namespace
I0316 08:09:35.606931  737621 kubeletfinalize.go:91] [kubelet-finalize] Assuming that kubelet client certificate rotation is enabled: found "/var/lib/kubelet/pki/kubelet-client-current.pem"
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
I0316 08:09:35.611075  737621 kubeletfinalize.go:135] [kubelet-finalize] Restarting the kubelet to enable client certificate rotation
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

  kubeadm join 10.0.16.71:6443 --token sewekg.e85zilfzi0v3vxf3 \
	--discovery-token-ca-cert-hash sha256:cd8b1e4f1b3311b5160c036cda22168247d30983f41702ed0f1e4b64a117eae1 \
	--control-plane --certificate-key 23911e0dbad42ca9c82419f332ffc9367fa1b3ca5c5ab74011026f96240b4e12

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.0.16.71:6443 --token sewekg.e85zilfzi0v3vxf3 \
	--discovery-token-ca-cert-hash sha256:cd8b1e4f1b3311b5160c036cda22168247d30983f41702ed0f1e4b64a117eae1
```

<br>

## 에러 종류
보통 5,6,9번에서 에러가 남 
특히 private 환경에서 k8s를 구성할때 이미지 문제가 난다면 5번
인증서 부분의 문제면 6번, 최종 네트워크 연결인 kubelet에서 에러가 난다면 9번에서 에러가 난다. 

그래서 로그를 확인하기 위해 init 명령어에 `--v=5` 옵션을 추가함!!
```
sudo kubeadm init --image-repository=10.0.16.62:8080/new --control-plane-endpoint=10.0.16.71:6443 --upload-certs --v=5
```

<br>


## 실시간 로그 확인 명령어
* 실시간 kubelet 로그 확인: `journalctl -u kubelet -f`
* 실시간 시스템 전체 로그 확인: `journalctl -xe -f`

![](https://velog.velcdn.com/images/jupiter-j/post/5aa38ded-74a8-45e7-8cb0-f1d80154b934/image.png)

