# Training Notes
Source: https://github.com/kodekloudhub/certified-kubernetes-administrator-course

## 1. Introduction
## 2. Core Concepts
### 2.1 Cluster Architecture
- **Worker nodes**: host application as containers
- **Master**: manage, plan, schedule, monitor nodes
- **ETCD cluster**: database (key-value)
- **kube-scheduler**: indentifies the right node to place a container on based on the container resource requirements, worker nodes capacity and any other policies/constraints (taints & toleration, node affinity rules)
- **Kube Controller Manager**
    - Node-Controller: onboarding nodes, handle unavailable nodes
    - Replication-Controller: insures desired number of containers are running
- **kube-apiserver**: orchestrating all operations within the cluster
- **Container Runtime Engine**: runs the containers
    - e.g.: Docker, ContainerD, Rocket
- **kubelet**: agent that runs on each node in a cluster, listens for instructions from the kube-apiserver, deploys/destroys containers on the nodes
- **kube-proxy**: ensures the necessary rules are in place on the worker nodes to allow the containers running on the to reach each other.

![](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/k8s-arch1.PNG?raw=true)

#### ETCD
- ETCD
    - distributed, reliable key-value store
    - simple, secure, fast
- Key-Value store
    - only stores keys and values
        - `PUT <key> <value>`
        - `GET <key>` -> returns `<value>`
- Gettings started
    - Download, extract, install
    - Run ETCD service: `./etcd`
    - Run CLI commands: `./etcdctl` 
- Setup using `kubeadm`
    - `etcd-master` pod will run in `kube-system` namespace
    - Run `etcdctl` commands in `etcd-master` pod: `kubectl exec etcd-master -n kube-system etcdctl get / --prefix -keys-only`
    - Settings:
        - Listens on: `--initial-advertise-peer-urls`
        - HA Environment (multiple master nodes): `--initial-cluster-controller-0`
- Operating ETCD
    - Configure ETCDCTL
        - Set API version: `export ETCDCTL_API=3`
        - Configure certificates to authenticate with ETCD API Server: 
            ```
            --cacert /etc/kubernetes/pki/etcd/ca.crt     
            --cert /etc/kubernetes/pki/etcd/server.crt     
            --key /etc/kubernetes/pki/etcd/server.key
            ```
        - Kubectl command: `kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" `

#### kube-apiserver
- Only component to directly interact with ETCD datastore (all other components use the kube-apiserver to interact with ETCD)
- Steps followed in the `kube-apiserver` when it is consumed by a `kubectl` command (or a POST request):
    - Example: create a new pod command
        1. Authenticate User
        2. Validate Request
        3. Retreive data: get requested information from ETCD
        4. Update ETCD: creates pod object in database, notifies user the pod has been created
        5. kube-scheduler: monitors API server, sees new pod, places new pod on right node through its kubelet, communicates back to API server
        6. Kubelet: creates pod and instructs container runtime engine to deploy the application image, update status back to API server
- Setup using `kubeadm`
    - `kube-apiserver-master` pod will run in `kube-system` namespace
    - Settings: `/etc/kubernetes/manifests/kube-apiserver.yaml`
        - `--etcd-cafile=/var/lib/kubernetes/ca.pem`
        - `--etcd-certfile=/var/lib/kubernetes/kubernetes.pem`
        - `--etcd-keyfile=/var/lib/kubernetes/kubernetes-key.pem`
        - `--kubelet-certificate-authority=/var/lib/kubernetes/ca.pem`
        - `--kubelet-client-certificate=/var/lib/kubernetes/kubernetes.pem`
        - `--kubelet-client-key=/var/lib/kubernetes/kubernetes-key.pem`
        - `--kubelet-https=true`
- Setup without `kubeadm`
    - Settings: `/etc/systemd/system/kube-apiserver.service`
    - View settings using process monitoring: `ps -aux | grep kube-apiserver`

#### Kube Controller Manager
- manages various controllers in Kubernetes ("brain behind Kubernetes")
- continuously monitors the state of various components within the system through the kube-apiserver
    - `node-controller`
        1. Watch Status: monitoring status of the nodes
            - Node monitor period = 5s
            - Node monitor grace period = 40s
            - Pod eviction timeout = 5m
        2. Remediate Situation
    - `replication-controller`: monitor state of replica-sets
    - `deployment-controller`
    - `namespace-controller`
    - `endpoint-controller`
    - `CronJob`
    - `job-controller`
    - `pv-protection-controller`
    - `service-account-controller`
    - `stateful-set`
    - `replica-set`
    - `pv-binder-controller`
- Setup using `kubeadm`
    - `kube-controller-manager-master` pod will run in `kube-system` namespace
    - Settings: `/etc/kubernetes/manifests/kube-controller-manager.yaml`
        - `--node-monitor-period=5s`
        - `--node-monitor-grace-period=40s`
        - `--pod-eviction-timeout=5m0s`
- Setup without `kubeadm`
    - Settings: `/etc/systemd/system/kube-controller-manager.service`
    - View settings using process monitoring: `ps -aux | grep kube-controller-manager`

#### kube-scheduler
- decides which pod goes on which node
- node schedule criteria:
    - Resource requirements and limits
        1. Filter nodes based on different resource requirements (CPU, memory)
        2. Rank nodes based on available resources
    - Tains and tolerations
    - Node selectors/affinity
- see section 3 (Scheduling)
- Setup using `kubeadm`
    - `kube-scheduler-master` pod will run in `kube-system` namespace
    Settings: `/etc/kubernetes/manifests/kube-scheduler.yaml`
- Setup without `kubeadm`
    - Settings: `/etc/systemd/system/kube-scheduler.service`
    - View settings using process monitoring: `ps -aux | grep kube-scheduler`

#### kubelet

### 2.2 API Primitives
### 2.3 Services & Other Network Primitives




## 3. Scheduling
## 4. Logging & Monitoring
## 5. Application Lifecycle Management
## 6. Cluster Maintenance
## 7. Security
## 8. Storage
## 9. Networking
## 10. Design and Install a Kubernetes Cluster
## 11. Install "Kubernetes the kubeadm way"
## 12. End to End Tests on a Kubernetes Cluster
## 13. Troubleshooting
## 14. Other Topics
## 15. Lightning Labs
## 16. Mock Exams
## 17. Course Conclusion