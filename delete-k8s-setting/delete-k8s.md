 # k8s 삭제
 public 환경에서 설치한 k8s를 삭제하는 과정을 정리함 

> 삭제되어야 하는 것들
> 1. 잔존한 pv,pvc -> deploy -> pod 삭제하기
> 2. cni 네트워크 구성 삭제 
> 3. kubeadm reset (여기서부터 시작)
> 4. containerd 삭제 
> 5. k8s 데이터 삭제 


<br>


* 디스크 공간 사용량 확인
```
[root@k8s-master-2 ~]# df -h
Filesystem                    Size  Used Avail Use% Mounted on
devtmpfs                      4.0M     0  4.0M   0% /dev
tmpfs                         3.8G     0  3.8G   0% /dev/shm
tmpfs                         1.6G  9.4M  1.5G   1% /run
/dev/vda4                      49G   34G   16G  68% /
/dev/vda3                     960M  170M  791M  18% /boot
/dev/vda2                     200M  7.1M  193M   4% /boot/efi
10.0.16.71:/data/cmp-nas/k8s   49G   34G   16G  68% /data/cmp-storage-k8s
tmpfs                         769M     0  769M   0% /run/user/0
shm                            64M     0   64M   0% /run/containerd/io.containerd.grpc.v1.cri/sandboxes/8c9d69891fb277dac8fe349389509ef37fae3de224d88d1451a5f3ade20cd7e2/shm
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/8c9d69891fb277dac8fe349389509ef37fae3de224d88d1451a5f3ade20cd7e2/rootfs
shm                            64M     0   64M   0% /run/containerd/io.containerd.grpc.v1.cri/sandboxes/305d1a64a736c9963ca6c7ea2b2f7f2bf2d03b03604050a349fe37048e7f9e6f/shm
shm                            64M     0   64M   0% /run/containerd/io.containerd.grpc.v1.cri/sandboxes/5fd79c42421880a8a32382f79444b079848a7c6c9c4275eced65b283e22667ab/shm
shm                            64M     0   64M   0% /run/containerd/io.containerd.grpc.v1.cri/sandboxes/78481a4c241d58864340b7b44750745b93b5667b6d7e9bfb5c8125ab19dfff11/shm
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/305d1a64a736c9963ca6c7ea2b2f7f2bf2d03b03604050a349fe37048e7f9e6f/rootfs
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/5fd79c42421880a8a32382f79444b079848a7c6c9c4275eced65b283e22667ab/rootfs
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/78481a4c241d58864340b7b44750745b93b5667b6d7e9bfb5c8125ab19dfff11/rootfs
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/0d21036db7d10b2562c4282dc0e8dba98f75da5b2a9a54becd8c091ca77a2d7c/rootfs
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/a2314e21319fcf4032ea08153b734d72a0ec7df87194b3a9b886e2f49cef6b19/rootfs
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/ebb6de4659547ac76a8db7ee359a199a7f185b9871c06a86d3839d3eeaf5f402/rootfs
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/89d8260f9f4a33608574baac4c4d11adc44c126df0a7f84784fd48e4cba56098/rootfs
tmpfs                         7.5G   12K  7.5G   1% /var/lib/kubelet/pods/08d6b196-6ba4-4547-a73b-660bfe91080e/volumes/kubernetes.io~projected/kube-api-access-jbwv5
shm                            64M     0   64M   0% /run/containerd/io.containerd.grpc.v1.cri/sandboxes/04a16e875f57322ec34f52a1dc36374b0ed73e03ec432d9bbf78c94fc1909809/shm
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/04a16e875f57322ec34f52a1dc36374b0ed73e03ec432d9bbf78c94fc1909809/rootfs
overlay                        49G   34G   16G  68% /run/containerd/io.containerd.runtime.v2.task/k8s.io/f15a69c8e52e8556492eaeb4da4fda7d8a35925cb835c69aed185c93368d48da/rootfs
```

* k8s reset
```
sudo kubeadm reset -f
```
![](https://velog.velcdn.com/images/jupiter-j/post/bb0b7e2d-1dc7-41a1-b1db-76c6c5fe91c4/image.png)

<br>

* 컨트롤 플레인 및 worker 노드의 Kubernetes 구성 제거
```
systemctl stop kubelet
kubeadm reset --cri-socket unix:///var/run/containerd/containerd.sock
rm -rf /etc/cni/net.d $HOME/.kube/config
```
<br>


* iptables, 네트워크 브릿지 설정 초기화
```
[root@k8s-worker-1 lib]# iptables -F && iptables -X && iptables -t nat -F && iptables -t nat -X
[root@k8s-worker-1 lib]# iptables -t mangle -F && iptables -t mangle -X && iptables -P FORWARD ACCEPT
```
<br>

* Kubernetes 노드에서 불필요한 이미지 및 컨테이너 정리
* containerd 및 kubelet 재시작으로 클린 상태 유지
* 디스크 공간 확보 및 환경 초기화
```
nerdctl rmi $(nerdctl images -a -q) -f && nerdctl container prune && systemctl restart kubelet && systemctl restart containerd && nerdctl system prune -a
```
> * `nerdctl rmi $(nerdctl images -a -q) -f`
	모든 이미지 목록(-a) 에서 이미지 ID만 출력(-q)
    위에서 얻은 모든 이미지 ID를 nerdctl rmi 명령어에 전달하여 강제 삭제 (-f)
> * `nerdctl container prune` : 실행 중이 않은 모든 컨테이너를 정리
> * `systemctl restart kubelet && systemctl restart containerd` : Kubelet과 containerd를 재시작하여 변경 사항 반영 및 정상 동작하도록 함.
> * `nerdctl system prune -a` : 불필요한 리소스(네트워크, 볼륨 등)까지 모두 정리 -a 옵션을 붙이면 사용하지 않는 이미지, 컨테이너, 볼륨, 네트워크 등 전부 삭제


⚠️ 현재 실행 중인 컨테이너는 정리되지 않지만, 정지된 컨테이너와 사용되지 않는 리소스는 모두 삭제됨.

<br>

```
[root@k8s-master-2 lib]# df -h
Filesystem                    Size  Used Avail Use% Mounted on
devtmpfs                      4.0M     0  4.0M   0% /dev
tmpfs                         3.8G     0  3.8G   0% /dev/shm
tmpfs                         1.6G  8.7M  1.5G   1% /run
/dev/vda4                      49G   33G   17G  67% /
/dev/vda3                     960M  170M  791M  18% /boot
/dev/vda2                     200M  7.1M  193M   4% /boot/efi
10.0.16.71:/data/cmp-nas/k8s   49G   33G   17G  67% /data/cmp-storage-k8s
tmpfs                         769M     0  769M   0% /run/user/0
```
![](https://velog.velcdn.com/images/jupiter-j/post/cdbffc56-ac5d-42d2-97c4-a85cb852894c/image.png)

<br>

* kubelet, kubeadm, kubectl 삭제
```
sudo dnf remove -y kubelet kubeadm kubectl
sudo rm -rf /etc/yum.repos.d/kubernetes.repo
```
* containerd 삭제
```
[root@k8s-master-2 lib]# sudo systemctl stop containerd
[root@k8s-master-2 lib]# sudo systemctl disable containerd
Removed "/etc/systemd/system/multi-user.target.wants/containerd.service".
[root@k8s-master-2 lib]# sudo rm -rf /usr/local/bin/containerd /usr/local/bin/containerd-shim* /usr/local/sbin/runc
[root@k8s-master-2 lib]# sudo rm -rf /etc/containerd /var/lib/containerd
[root@k8s-master-2 lib]# sudo rm -rf /opt/cni/bin /etc/cni /var/lib/cni
```

* k8s 데이터 삭제
```
sudo rm -rf ~/.kube
sudo rm -rf /etc/kubernetes/
sudo rm -rf /var/lib/kubelet
sudo rm -rf /var/lib/etcd
```