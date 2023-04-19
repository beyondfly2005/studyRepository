
> https://www.cnblogs.com/hujunwei/p/15893319.html
> https://blog.csdn.net/qq_69243343/article/details/126256942
> 

K8S的架构，略微有一点复杂，我们简单来看一下。

一个K8S系统，通常称为一个K8S集群（Cluster）。

这个集群主要包括两个部分：

- 一个Master节点（主节点）

  Master节点主要还是负责管理和控制。

- 一群Node节点（计算节点）
  Node节点是工作负载节点，里面是具体的容器。

### Master节点：

Master节点包括API Server、Scheduler、Controller manager、etcd。

- API Server是整个系统的对外接口，供客户端和其它组件调用，相当于“营业厅”。
-	Scheduler负责对集群内部的资源进行调度，相当于“调度室”。
-	Controller manager负责管理控制器，相当于“大总管”。

### Node节点

Node节点包括Docker、kubelet、kube-proxy、Fluentd、kube-dns（可选），还有就是Pod。

- Docker，不用说了，创建容器的。
- Kubelet，主要负责监视指派到它所在Node上的Pod，包括创建、修改、监控、删除等。
- Kube-proxy，主要负责为Pod对象提供代理。
- Fluentd，主要负责日志收集、存储与查询。