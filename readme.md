## practice in k8s and kind

**Cluster from project "k8slab"**
---------

*Output from:*  **kubectl describe pod my-pod**



Name:             my-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             k8s-worker2/172.19.0.3
Start Time:       Tue, 13 Dec 2022 16:03:22 +0300
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.1.5
IPs:
  IP:  10.244.1.5
Containers:
  nginx:
    Container ID:   containerd://ee70381f5ba1a2de6a2c02ea4a2544d13873a45b78e5b4f9405aac9e8b5ee96f
    Image:          nginx:1.12
    Image ID:       docker.io/library/nginx@sha256:72daaf46f11cc753c4eab981cbf869919bd1fee3d2170a2adeac12400f494728
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 13 Dec 2022 16:03:41 +0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-lh72b (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-lh72b:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s


Events:
  Type    Reason     Age   From               Message
  Normal  Scheduled  16m   default-scheduler  Successfully assigned default/my-pod to **k8s-worker2**
  Normal  Pulling    16m   kubelet            Pulling image "nginx:1.12"
  Normal  Pulled     15m   kubelet            Successfully pulled image "nginx:1.12" in 18.032222807s
  Normal  Created    15m   kubelet            Created container nginx
  Normal  Started    15m   kubelet            Started container nginx
  



-----------

  **kubectl get po --show-labels**
  
| NAME                | READY | STATUS  | RESTARTS | AGE | LABELS       |
|---------------------|-------|---------|----------|-----|--------------|
| my-pod              | 1/1   | Running | 0        | 49m | <none>       |
| my-replicaset-52zt9 | 1/1   | Running | 0        | 64s | app=my-app-1 |
| my-replicaset-8x8rj | 1/1   | Running | 0        | 64s | app=my-app-1 |

-----------
### scale number of replicas in "my-replicaset"

**kubectl scale replicaset my-replicaset --replicas=3** \
*replicaset.apps/my-replicaset scaled*  \

**root@book:/home/nick/k8slab2# kubectl get po -w**  \

NAME                  READY   STATUS    RESTARTS   AGE  
my-pod                1/1     Running   0          93m  
my-replicaset-52zt9   1/1     Running   0          45m  
my-replicaset-8x8rj   1/1     Running   0          45m  
my-replicaset-sxs4r   1/1     Running   0          32s  
  
 -----------
 ### Delete all
 **kubectl delete all --all**
pod "my-pod" deleted
pod "my-replicaset-52zt9" deleted
pod "my-replicaset-8x8rj" deleted
service "kubernetes" deleted
replicaset.apps "my-replicaset" deleted


----------
### Change image in deployment 
**kubectl set image deployment my-deployment '*=nginx:1.14'**  


----------
### Deployment with requests and limits
Р/л можно посмотреть наглядно с помощью **systemd-cdls**

kubelet.slice  
  │   ├─kubelet.service  
  │   │ └─22018 /usr/bin/kubelet --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf -->  
  │   └─kubelet-kubepods.slice  
  │     ├─kubelet-kubepods-besteffort.slice  
  │     │ └─kubelet-kubepods-besteffort-pod6de31acc_3b6d_4fe3_bd53_2cc9ddcd1b0e.slice  
  │     │   ├─cri-containerd-8447f8fa852e0fb592bb4bc23c5f46f5c0ce260c63fab167629176e7695d5209.scope …  
  │     │   │ └─22240 /pause  
  │     │   └─cri-containerd-93b19f093cb98ef412b77b3299cf1135433922cd0c52a72d77de7ea1cee387c3.scope …  
  │     │     └─78694 /usr/local/bin/kube-proxy --config=/var/lib/kube-proxy/config.conf --hostname-override=k8s-worker  
  │     ├─kubelet-kubepods-burstable.slice  
  │     │ ├─kubelet-kubepods-burstable-pod2c93d563_011e_4905_97eb_cb4cd37bd556.slice   
  │     │ │ ├─cri-containerd-2e87b35582c24ad7b454e279dbb77e988dbac74eba5c468842df1623948c1676.scope …  
  │     │ │ │ ├─91669 nginx: master process nginx -g daemon off;  
  │     │ │ │ └─91817 nginx: worker process  
  │     │ │ └─cri-containerd-b81aab8859a0cd57586808576a21b70cab39330d6c147efd9b6e5236e0f33c7b.scope …  
  │     │ │   └─91577 /pause  
  │     │ ├─kubelet-kubepods-burstable-pod42a21974_5b14_4388_ae74_4efeaf94c007.slice  
  │     │ │ ├─cri-containerd-9bd8c01d73675a7082b75a19b2e60d0ec41191a6d8081868cf08ea75209b8ec6.scope …  
  │     │ │ │ ├─91751 nginx: master process nginx -g daemon off;  
  │     │ │ │ └─91816 nginx: worker process  
  │     │ │ └─cri-containerd-e0cc8c12abec623f09d77a189d0fba622c8e6d36df994b887a343a68179a9570.scope …  
  │     │ │   └─91637 /pause  
  │     │ └─kubelet-kubepods-burstable-pod7511d689_6634_4348_89ee_ce03934f78e7.slice  
  │     │   ├─cri-containerd-424a77da70d80e4b029c7ab53ea6983bae57d627ee0a134f1b150ba0d520dc32.scope …  
  │     │   │ └─91568 /pause  
  │     │   └─cri-containerd-51a6855959e7219d800013b80144136848590deca89cd22b4030b7586e90506f.scope …  
  │     │     ├─91704 nginx: master process nginx -g daemon off;  
  │     │     └─91818 nginx: worker process  
  │     └─kubelet-kubepods-pod25cabf10_5d70_47ff_a8ce_d96910a14182.slice  
  │       ├─cri-containerd-9b47afda2e27f5e211a9ec35020759b3124e24710052be1ae53be8c5abbd12d9.scope …  
  │       │ └─22247 /pause  
  │       └─cri-containerd-cf9f031cf67854e2f012f7632b0884dbd9386c581435afc5999a13c5204e609f.scope …  
  │         └─22606 /bin/kindnetd  


QoS classes:

BestEfford - когда р/л не заданы    
Guaranteed - когда р/л одинаковые
Burstable - когда р/л отличаются

При нехватки памяти на ноде и ОМKiller, и куб будет сначала эвакуировать BestEfford поды, затем Burstable, только потом Guaranteed