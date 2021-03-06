# Kubernetes architecture
![](images/archi2.png)

![architecture.png](images/architecture.png)

### Master Node
Đầu tiên, ta có Master Node. Nó chịu trách nhiệm cho việc quản lý Kubernetes Cluster. Nó có 3 components bên trong để quản lý các việc communication(kube-apiserver), scheduling(kube-scheduler) và controller(kube-controller-manager).
* Kube API Server như các tên của nó cho phép ta tương tác với Kubernetes API. Nó là front end của Kubernetes control plane.
* Scheduler sẽ quan sát những Pods chưa được gán cho node cụ thể nào, sau đó gán cho Pod chạy trên 1 node cụ thế
* Controller Manager chạy các controllers. Chúng là các background threads chạy các task bên trong cluster. Controller bao gồm nhiều vai trò khác nhau, nhưng tất cả được compiled thành một single binary. Những vai trò của controllers bao gồm.
  - Node controller chịu trách nhiệm cho trạng thái của worker(worker state)
  - Replication controller chịu trách nhiệm cho việc đảm bảo duy trì (maintaining) đúng số lượng của Pods
  - End-point Controller, Kết nối services và Pods với nhau.
  - Service account và token controllers quản lý access management.

### etcd
etcd là một simple distributed key value store. Kubernetes sử dụng etcd như là database của nó và lưu trữ tất cả cluster data ở đây.

Một vài thông tin có thể được lưu trữ như là, job scheduling info, Pod details, stage information, etc, ...

### kubectl
Ta tương tác(interact) với Master Node sử dụng kubectl application, kubectl là command line interface cho Kubernetes.

Kubectl có config file được gọi là kubeconfig. File này có nhưng thông tin như: server information, authentication infomation dể access API Server.

### Worker node
Worker node là node nơi mà ứng dụng(application) được triển khai. Worker node giao tiếp (communicate back) với master node.

* Giao tiếp với worker node được xử lý bởi Kubelet. Kubelet là một agent giao tiếp với API Server để xem liệu Pods đã được cho vào Node hay chưa.
- kubelet execute Pod containers thông qua container engine.
- Nó mount và runs Pod volume, secrets. Nó xem trạng thái của Pod của Node và phản hồi về 		Master
- Docker là một container native plaform làm việc với kubelet để chạy containers trên các Nodes

![](images/worker.png)
### kubeproxy
kube-proxy là Network Proxy và load balancer cho dịch vụ, trên một single worker node. Nó đảm nhiệm network routing cho TCP và UDP packets, và thực hiện connection forwarding.

Một hoặc nhiều containers sẽ được gói trong Pod. Pod là unit nhỏ nhat có thể được scheduled như một deployment trong Kubernetes. Nhóm containers trong pod dùng chung storage, Linux name space, IP addresses, ...

Một khi Pod được deployed và running, kubelet process giao tiếp với Pods để check state và health. kube-proxy sẽ route any packets tới Pods từ resources khác muốn communicate vói chúng. Worker node có thể được expose tới internet thông qua load balancer. Traffic đi vào các node cũng sẽ được kube-proxy đảm nhiệm.
