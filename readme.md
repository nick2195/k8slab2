## practice in k8s and kind

**Cluster from project "k8slab"**
---------

*Output from:  **kubectl describe pod my-pod**



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


|NAME  | 
---               READY|   STATUS  |  RESTARTS |  AGE  | LABELS
|my-pod                1/1     Running   0          49m   <none>
|my-replicaset-52zt9   1/1     Running   0          64s   app=my-app-1
|my-replicaset-8x8rj   1/1     Running   0          64s   app=my-app-1

