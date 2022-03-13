# Training Notes
Source: https://github.com/kodekloudhub/certified-kubernetes-administrator-course

## 1. Introduction
## 2. Core Concepts
### 2.1 Cluster Architecture
- **Worker nodes**: host application as containers
- **Master**: manage, plan, schedule, monitor nodes
- **ETCD cluster**: database (key-value)
- **kube-scheduler**: indentified the right node to place a container on based on the container resource requirements, worker nodes capacity and any other policies/constraints (taints & toleration, node affinity rules)
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
- Operating ETC

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