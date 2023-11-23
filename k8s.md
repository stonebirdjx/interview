<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [kubectl 创建 Pod 背后到底发生了什么？](#kubectl-%E5%88%9B%E5%BB%BA-pod-%E8%83%8C%E5%90%8E%E5%88%B0%E5%BA%95%E5%8F%91%E7%94%9F%E4%BA%86%E4%BB%80%E4%B9%88)
- [pod的终止过程](#pod%E7%9A%84%E7%BB%88%E6%AD%A2%E8%BF%87%E7%A8%8B)
- [kubelet的功能、作用是什么？（重点，经常会问）](#kubelet%E7%9A%84%E5%8A%9F%E8%83%BD%E4%BD%9C%E7%94%A8%E6%98%AF%E4%BB%80%E4%B9%88%E9%87%8D%E7%82%B9%E7%BB%8F%E5%B8%B8%E4%BC%9A%E9%97%AE)
- [k8s中命名空间的作用是什么？](#k8s%E4%B8%AD%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4%E7%9A%84%E4%BD%9C%E7%94%A8%E6%98%AF%E4%BB%80%E4%B9%88)
- [Kubernetes Proxy API作用](#kubernetes-proxy-api%E4%BD%9C%E7%94%A8)
- [pod是什么？](#pod%E6%98%AF%E4%BB%80%E4%B9%88)
- [pod的原理是什么？](#pod%E7%9A%84%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88)
- [pod有什么特点](#pod%E6%9C%89%E4%BB%80%E4%B9%88%E7%89%B9%E7%82%B9)
- [pod的重启策略有哪些？](#pod%E7%9A%84%E9%87%8D%E5%90%AF%E7%AD%96%E7%95%A5%E6%9C%89%E5%93%AA%E4%BA%9B)
- [pod的生命周期有哪几种？](#pod%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D)
- [pod一直处于pending状态一般有哪些情况，怎么排查？](#pod%E4%B8%80%E7%9B%B4%E5%A4%84%E4%BA%8Epending%E7%8A%B6%E6%80%81%E4%B8%80%E8%88%AC%E6%9C%89%E5%93%AA%E4%BA%9B%E6%83%85%E5%86%B5%E6%80%8E%E4%B9%88%E6%8E%92%E6%9F%A5)
- [k8s针对pod资源对象的健康监测机制?](#k8s%E9%92%88%E5%AF%B9pod%E8%B5%84%E6%BA%90%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%81%A5%E5%BA%B7%E7%9B%91%E6%B5%8B%E6%9C%BA%E5%88%B6)
- [pod的共享资源？](#pod%E7%9A%84%E5%85%B1%E4%BA%AB%E8%B5%84%E6%BA%90)
- [pause容器作用是什么？](#pause%E5%AE%B9%E5%99%A8%E4%BD%9C%E7%94%A8%E6%98%AF%E4%BB%80%E4%B9%88)
- [pod的定义中有个command和args参数](#pod%E7%9A%84%E5%AE%9A%E4%B9%89%E4%B8%AD%E6%9C%89%E4%B8%AAcommand%E5%92%8Cargs%E5%8F%82%E6%95%B0)
- [标签及标签选择器是什么，如何使用？](#%E6%A0%87%E7%AD%BE%E5%8F%8A%E6%A0%87%E7%AD%BE%E9%80%89%E6%8B%A9%E5%99%A8%E6%98%AF%E4%BB%80%E4%B9%88%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8)
- [就绪探针（ReadinessProbe探针）与存活探针（livenessProbe探针）区别是什么？](#%E5%B0%B1%E7%BB%AA%E6%8E%A2%E9%92%88readinessprobe%E6%8E%A2%E9%92%88%E4%B8%8E%E5%AD%98%E6%B4%BB%E6%8E%A2%E9%92%88livenessprobe%E6%8E%A2%E9%92%88%E5%8C%BA%E5%88%AB%E6%98%AF%E4%BB%80%E4%B9%88)
- [探针执行方式](#%E6%8E%A2%E9%92%88%E6%89%A7%E8%A1%8C%E6%96%B9%E5%BC%8F)
- [service的域名解析格式、pod的域名解析格式](#service%E7%9A%84%E5%9F%9F%E5%90%8D%E8%A7%A3%E6%9E%90%E6%A0%BC%E5%BC%8Fpod%E7%9A%84%E5%9F%9F%E5%90%8D%E8%A7%A3%E6%9E%90%E6%A0%BC%E5%BC%8F)
- [service的类型有哪几种](#service%E7%9A%84%E7%B1%BB%E5%9E%8B%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D)
- [service、endpoint、kube-proxys三种的关系是什么？](#serviceendpointkube-proxys%E4%B8%89%E7%A7%8D%E7%9A%84%E5%85%B3%E7%B3%BB%E6%98%AF%E4%BB%80%E4%B9%88)
- [无头service和普通的service有什么区别，无头service使用场景是什么？](#%E6%97%A0%E5%A4%B4service%E5%92%8C%E6%99%AE%E9%80%9A%E7%9A%84service%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%E6%97%A0%E5%A4%B4service%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF%E6%98%AF%E4%BB%80%E4%B9%88)
- [deployment怎么扩容或缩容？](#deployment%E6%80%8E%E4%B9%88%E6%89%A9%E5%AE%B9%E6%88%96%E7%BC%A9%E5%AE%B9)
- [deployment的更新升级策略有哪些？](#deployment%E7%9A%84%E6%9B%B4%E6%96%B0%E5%8D%87%E7%BA%A7%E7%AD%96%E7%95%A5%E6%9C%89%E5%93%AA%E4%BA%9B)
- [简述一下deployment的更新过程?](#%E7%AE%80%E8%BF%B0%E4%B8%80%E4%B8%8Bdeployment%E7%9A%84%E6%9B%B4%E6%96%B0%E8%BF%87%E7%A8%8B)
- [deployment的回滚使用什么命令](#deployment%E7%9A%84%E5%9B%9E%E6%BB%9A%E4%BD%BF%E7%94%A8%E4%BB%80%E4%B9%88%E5%91%BD%E4%BB%A4)
- [存储卷分类，作用分别是什么?](#%E5%AD%98%E5%82%A8%E5%8D%B7%E5%88%86%E7%B1%BB%E4%BD%9C%E7%94%A8%E5%88%86%E5%88%AB%E6%98%AF%E4%BB%80%E4%B9%88)
- [pv的访问模式有哪几种](#pv%E7%9A%84%E8%AE%BF%E9%97%AE%E6%A8%A1%E5%BC%8F%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D)
- [pv的回收策略有哪几种](#pv%E7%9A%84%E5%9B%9E%E6%94%B6%E7%AD%96%E7%95%A5%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D)
- [在pv的生命周期中，一般有几种状态](#%E5%9C%A8pv%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%B8%AD%E4%B8%80%E8%88%AC%E6%9C%89%E5%87%A0%E7%A7%8D%E7%8A%B6%E6%80%81)
- [DaemonSet资源对象的特性？](#daemonset%E8%B5%84%E6%BA%90%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%89%B9%E6%80%A7)
- [如何停机维护又不能影响业务应用](#%E5%A6%82%E4%BD%95%E5%81%9C%E6%9C%BA%E7%BB%B4%E6%8A%A4%E5%8F%88%E4%B8%8D%E8%83%BD%E5%BD%B1%E5%93%8D%E4%B8%9A%E5%8A%A1%E5%BA%94%E7%94%A8)
- [etcd集群节点可以设置为偶数个吗，为什么要设置为基数个呢？](#etcd%E9%9B%86%E7%BE%A4%E8%8A%82%E7%82%B9%E5%8F%AF%E4%BB%A5%E8%AE%BE%E7%BD%AE%E4%B8%BA%E5%81%B6%E6%95%B0%E4%B8%AA%E5%90%97%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E8%AE%BE%E7%BD%AE%E4%B8%BA%E5%9F%BA%E6%95%B0%E4%B8%AA%E5%91%A2)
- [etcd集群节点之间是怎么同步数据的？](#etcd%E9%9B%86%E7%BE%A4%E8%8A%82%E7%82%B9%E4%B9%8B%E9%97%B4%E6%98%AF%E6%80%8E%E4%B9%88%E5%90%8C%E6%AD%A5%E6%95%B0%E6%8D%AE%E7%9A%84)
- [kube-proxy原理?](#kube-proxy%E5%8E%9F%E7%90%86)
- [简述kube-proxy ipvs和iptables的异同?](#%E7%AE%80%E8%BF%B0kube-proxy-ipvs%E5%92%8Ciptables%E7%9A%84%E5%BC%82%E5%90%8C)
- [K8S 资源限制 QoS？](#k8s-%E8%B5%84%E6%BA%90%E9%99%90%E5%88%B6-qos)
- [k8s数据持久化的方式有哪些？](#k8s%E6%95%B0%E6%8D%AE%E6%8C%81%E4%B9%85%E5%8C%96%E7%9A%84%E6%96%B9%E5%BC%8F%E6%9C%89%E5%93%AA%E4%BA%9B)
- [K8s中镜像的下载策略是什么？](#k8s%E4%B8%AD%E9%95%9C%E5%83%8F%E7%9A%84%E4%B8%8B%E8%BD%BD%E7%AD%96%E7%95%A5%E6%98%AF%E4%BB%80%E4%B9%88)
- [kubelet 监控 Node 节点资源使用是通过什么组件来实现的？](#kubelet-%E7%9B%91%E6%8E%A7-node-%E8%8A%82%E7%82%B9%E8%B5%84%E6%BA%90%E4%BD%BF%E7%94%A8%E6%98%AF%E9%80%9A%E8%BF%87%E4%BB%80%E4%B9%88%E7%BB%84%E4%BB%B6%E6%9D%A5%E5%AE%9E%E7%8E%B0%E7%9A%84)
- [kubernetes服务发现？](#kubernetes%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0)
- [简述ETCD及其特点?](#%E7%AE%80%E8%BF%B0etcd%E5%8F%8A%E5%85%B6%E7%89%B9%E7%82%B9)
- [简述Kubernetes初始化容器（init container）?](#%E7%AE%80%E8%BF%B0kubernetes%E5%88%9D%E5%A7%8B%E5%8C%96%E5%AE%B9%E5%99%A8init-container)
- [简述Kubernetes自动扩容HPA机制?](#%E7%AE%80%E8%BF%B0kubernetes%E8%87%AA%E5%8A%A8%E6%89%A9%E5%AE%B9hpa%E6%9C%BA%E5%88%B6)
- [简述Kubernetes Service分发后端的策略?](#%E7%AE%80%E8%BF%B0kubernetes-service%E5%88%86%E5%8F%91%E5%90%8E%E7%AB%AF%E7%9A%84%E7%AD%96%E7%95%A5)
- [简述Kubernetes Scheduler作用及实现原理?](#%E7%AE%80%E8%BF%B0kubernetes-scheduler%E4%BD%9C%E7%94%A8%E5%8F%8A%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
- [简述Kubernetes Scheduler使用哪两种算法将Pod绑定到worker节点](#%E7%AE%80%E8%BF%B0kubernetes-scheduler%E4%BD%BF%E7%94%A8%E5%93%AA%E4%B8%A4%E7%A7%8D%E7%AE%97%E6%B3%95%E5%B0%86pod%E7%BB%91%E5%AE%9A%E5%88%B0worker%E8%8A%82%E7%82%B9)
- [简述Kubernetes准入机制?](#%E7%AE%80%E8%BF%B0kubernetes%E5%87%86%E5%85%A5%E6%9C%BA%E5%88%B6)
- [简述Kubernetes网络模型?](#%E7%AE%80%E8%BF%B0kubernetes%E7%BD%91%E7%BB%9C%E6%A8%A1%E5%9E%8B)
- [简述Kubernetes CNI模型?](#%E7%AE%80%E8%BF%B0kubernetes-cni%E6%A8%A1%E5%9E%8B)
- [简述Kubernetes PV和PVC?](#%E7%AE%80%E8%BF%B0kubernetes-pv%E5%92%8Cpvc)
- [简述Kubernetes CSI模型?](#%E7%AE%80%E8%BF%B0kubernetes-csi%E6%A8%A1%E5%9E%8B)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# kubectl 创建 Pod 背后到底发生了什么？

kubectl

- 验证和生成器：kubectl 首先会执行一些客户端验证操作，以确保不合法的请求（例如，创建不支持的资源或使用格式错误的镜像名称）将会快速失败，也不会发送给 kube-apiserver。通过减少不必要的负载来提高系统性能。 kubectl 使用生成器（generators）来构造 HTTP 请求。生成器是一个用来处理序列化的抽象概念。
- 客户端身份认证：在发送 HTTP 请求之前还要进行客户端认证，用户凭证保存在 kubeconfig 文件中

apiserver

- 认证：为了执行相应的功能，kube-apiserver 需要能够验证请求者是合法的，这个过程被称为认证。
- 授权：到此证明了我们是合法的，但我们有权执行此操作吗？毕竟身份和权限不是一回事。为了进行后续的操作，kube-apiserver 还要对用户进行授权。
- 准入控制: 其他组件对于应不应该允许发生的事情还是很有意见的,所以这个请求还需要通过 Admission Controller 所控制的一个 准入控制链 的层层考验.`SecurityContextDeny`、`ResourceQuota` 及 `LimitRanger`这三个准入控制器。

etcd

- 存储：kube-apiserver 将对 HTTP 请求进行反序列化，然后利用得到的结果构建运行时对象（有点像 kubectl 生成器的逆过程），并保存到 etcd 中。

初始化

- 关联控制器： 在一个资源对象被持久化到数据存储之后，apiserver 还无法完全看到或调度它，在此之前还要执行一系列 Initializers。Initializers 是一种与资源类型相关联的控制器，

controller

- 控制循环：
- Informers：Controller 是如何访问和修改这些资源对象的呢？事实上 Kubernetes 是通过 Informer 机制来解决这个问题的。Infomer 是一种模式，它允许 Controller 查找缓存在本地内存中的数据(这份数据由 Informer 自己维护)并列出它们感兴趣的资源。
- Scheduler：Scheduler 作为一个独立的组件运行在集群控制平面上，工作方式与其他 Controller 相同：监听实际并将系统状态调整到期望的状态

Kubelet

- pod同步：
- CRI 与 pause 容器
- CNI 和 Pod 网络
- 跨主机容器网络：通常情况下使用 overlay 网络来进行跨主机容器通信，这是一种动态同步多个主机间路由的方法。
- 容器启动

> 验证资源 -> 封装请求 -> apiserver认证、鉴权 -> 资源准入控制 -> etcd存储 -> controller -> schedule -> kubelet

# pod的终止过程

验证资源 -> 封装请求 -> apiserver认证、鉴权 -> apiserver发送删除pod对象的命令 -> 将pod标记为terminating状态 -> 

- endpoint控制器监控到pod对象的关闭行为时将其从所有匹配到此endpoint的server资源endpoint列表中删除； ->
- 如果当前pod对象定义了preStop钩子处理器，则在其被标记为terminating后会意同步的方式启动执行；->
- 宽限期结束后，若pod中还存在运行的进程，那么pod对象会收到立即终止的信息；

# kubelet的功能、作用是什么？（重点，经常会问）

- 节点管理 : kubelet启动时会向api-server进行注册，然后会定时的向api-server汇报本节点信息状态，资源使用状态等，这样master就能够知道node节点的资源剩余，节点是否失联等等相关的信息了。master知道了整个集群所有节点的资源情况，这对于 pod 的调度和正常运行至关重要。
- pod管理：kubelet负责维护node节点上pod的生命周期，当kubelet监听到master的下发到自己节点的任务时，比如要创建、更新、删除一个pod，kubelet 就会通过CRI（容器运行时接口）插件来调用不同的容器运行时来创建、更新、删除容器；
- 容器健康检查：pod中可以定义启动探针、存活探针、就绪探针等3种，我们最常用的就是存活探针、就绪探针，kubelet 会定期调用容器中的探针来检测容器是否存活，是否就绪，如果是存活探针，则会根据探测结果对检查失败的容器进行相应的重启策略；
- Metrics Server资源监控：在node节点上部署Metrics Server用于监控node节点、pod的CPU、内存、文件系统、网络使用等资源使用情况，而kubelet则通过Metrics Server获取所在节点及容器的上的数据。

# k8s中命名空间的作用是什么？

namespace是kubernetes系统中的一种非常重要的资源，namespace的主要作用是用来实现多套环境的资源隔离，或者说是多租户的资源隔离。

可以通过k8s的授权机制，将不同的namespace交给不同的租户进行管理，这样就实现了多租户的资源隔离，还可以结合k8s的资源配额机制，限定不同的租户能占用的资源，例如CPU使用量、内存使用量等等来实现租户可用资源的管理。

# Kubernetes Proxy API作用

作用是代理rest请求；

Kubernets API server 将接收到的rest请求转发到某个node上的kubelet守护进程的rest接口，由该kubelet进程负责响应。

# pod是什么？

k8s并不直接处理容器，而是使用多个容器共存的理念，这组容器就叫做pod。

pod是k8s中可以创建和管理的最小单元，是资源对象模型中由用户创建或部署的最小资源对象模型。

# pod的原理是什么？

在微服务的概念里，一般的，一个容器会被设计为运行一个进程，除非进程本身产生子进程，

这样，由于不能将多个进程聚集在同一个单独的容器中，所以需要一种更高级的结构将容器绑定在一起，并将它们作为一个单元进行管理，这就是k8s中pod的背后原理。

# pod有什么特点

- k8s会为每个pod分配一个集群内部唯一的IP地址，所以每个pod都拥有自己的IP地址、主机名、进程等；
- 每一个pod都有一个特殊的被称为"根容器"的pause容器，也称info容器，pause容器对应的镜像属于k8s平台的一部分，除了pause容器，每个pod还包含一个或多个跑业务相关组件的应用容器；
- 一个pod中的容器共享network命名空间；

# pod的重启策略有哪些？

pod重启容器策略是指针对pod内所有容器的重启策略，不是重启pod，其可以通过restartPolicy字段配置pod重启容器的策略，如下：

- Always: 当容器终止退出后，总是重启容器，默认策略就是Always。
- OnFailure: 当容器异常退出，退出状态码非0时，才重启容器。
- Never: 当容器终止退出，不管退出状态码是什么，从不重启容器。

# pod的生命周期有哪几种？

Pending（挂起）：API server已经创建pod，但是该pod还有一个或多个容器的镜像没有创建，包括正在下载镜像的过程；

Running（运行中）：Pod内所有的容器已经创建，且至少有一个容器处于运行状态、正在启动括正在重启状态；

Succeed（成功）：Pod内所有容器均已退出，且不会再重启；

Failed（失败）：Pod内所有容器均已退出，且至少有一个容器为退出失败状态

Unknown（未知）：某于某种原因apiserver无法获取该pod的状态，可能由于网络通行问题导致；

Eviction(被驱逐)： 意思是当节点出现异常时，为了保证工作负载的可用性，kubernetes将有相应的机制驱逐该节点上的Pod。pod写入的文件太大时会被驱逐。

# pod一直处于pending状态一般有哪些情况，怎么排查？

一个pod一开始创建的时候，它本身就是会处于pending状态，这时可能是正在拉取镜像，正在创建容器的过程。

- 调度器调度失败。
- 正在拉取镜像
- pvc、pv无法动态创建。
- 节点资源不够
- ResourceQuate、LimitRange

# k8s针对pod资源对象的健康监测机制?

提供了三类probe（探针）来执行对pod的健康监测：

- LivenessProbe（存活探针）: 可以根据用户自定义规则来判定pod是否健康，用于判断容器是否处于Running状态，
- ReadinessProbe（就绪探针）：同样是可以根据用户自定义规则来判断pod是否健康，容器服务是否可用（Ready），如果探测失败，控制器会将此pod从对应service的endpoint列表中移除，从此不再将任何请求调度到此Pod上，直到下次探测成功;
- startupProbe （启动探针）: 启动检查机制，应用一些启动缓慢的业务，避免业务长时间启动而被上面两类探针kill掉，

# pod的共享资源？

- PID 命名空间：Pod 中的不同应用程序可以看到其他应用程序的进程 ID；
- 网络命名空间：Pod 中的多个容器能够访问同一个IP和端口范围；
- IPC 命名空间：Pod 中的多个容器能够使用 SystemV IPC 或 POSIX 消息队列进行通信；
- UTS 命名空间：Pod 中的多个容器共享一个主机名；
- Volumes（共享存储卷）：Pod 中的各个容器可以访问在 Pod 级别定义的 Volumes；

# pause容器作用是什么？

每个pod里运行着一个特殊的被称之为pause的容器，也称根容器，而其他容器则称为业务容器；

创建pause容器主要是为了为业务容器提供 Linux命名空间，共享基础：包括 pid、icp、net 等，以及启动 init 进程，并收割僵尸进程；

# pod的定义中有个command和args参数

command参数用于指定容器的启动命令列表，如果不指定，则默认使用Dockerfile打包时的启动命令，args参数用于容器的启动命令需要的参数列表；

# 标签及标签选择器是什么，如何使用？

标签是键值对类型，标签可以附加到任何资源对象上，主要用于管理对象，查询和筛选。

在deployment中，在pod模板中定义pod的标签，然后在deployment定义标签选择器，这样就通过标签选择器来选择哪些pod是受其控制的，service也是通过标签选择器来关联哪些pod最后其服务后端pod。

# 就绪探针（ReadinessProbe探针）与存活探针（livenessProbe探针）区别是什么？

- 存活探针是将检查失败的容器杀死，创建新的启动容器来保持pod正常工作；
- 就绪探针是，当就绪探针检查失败，并不重启容器，而是将pod移出endpoint，就绪探针确保了service中的pod都是可用的，确保客户端只与正常的pod交互并且客户端永远不会知道系统存在问题。

# 探针执行方式

kubernetes提供了3种探测容器的存活探针，如下：

- httpGet：通过容器的IP、端口、路径发送http 请求，返回200-400范围内的状态码表示成功。
- exec：在容器内执行shell命令，根据命令退出状态码是否为0进行判断，0表示健康，非0表示不健康。
- TCPSocket：与容器的IP、端口建立TCP Socket链接，能建立则说明探测成功，不能建立则说明探测失败

# service的域名解析格式、pod的域名解析格式

- service的DNS域名表示格式为<servicename>.<namespace>.svc.<clusterdomain>，servicename是service的名称，namespace是service所处的命名空间，clusterdomain是k8s集群设置的域名后缀，一般默认为 cluster.local
- pod的DNS域名格式为：<pod-ip>.<namespace>.pod.<clusterdomain> ，其中，pod-ip需要使用-将ip直接的点替换掉，namespace为pod所在的命名空间，clusterdomain是k8s集群设置的域名后缀，一般默认为 cluster.local ，
  - 演示如下：10-244-1-223.default.pod.cluster.local
  - 对于deployment、daemonsets等创建的pod，还还可以通过<pod-ip>.<deployment-name>.<namespace>.svc.<clusterdomain> 这样的域名访问。

# service的类型有哪几种

- ClusterIP：表示service仅供集群内部使用，默认值就是ClusterIP类型
- NodePort：表示service可以对外访问应用，会在每个节点上暴露一个端口，这样外部浏览器访问地址为：任意节点的IP：NodePort就能连上service了
- LoadBalancer：表示service对外访问应用，这种类型的service是公有云环境下的service，此模式需要外部云厂商的支持，需要有一个公网IP地址
- ExternalName：这种类型的service会把集群外部的服务引入集群内部，这样集群内直接访问service就可以间接的使用集群外部服务了

# service、endpoint、kube-proxys三种的关系是什么？

- service：在kubernetes中，service是一种为一组功能相同的pod提供单一不变的接入点的资源。
- endpoint：service维护一个叫endpoint的资源列表，endpoint资源对象保存着service关联的pod的ip和端口。
- kube-proxy：kube-proxy运行在node节点上，在Node节点上实现Pod网络代理，维护网络规则和四层负载均衡工作

> kube-proxy会监听api-server中从而获取service和endpoint的变化情况，创建并维护路由规则以提供服务IP和负载均衡功能。

# 无头service和普通的service有什么区别，无头service使用场景是什么？

无头service没有cluster ip，在定义service时将 service.spec.clusterIP：None，就表示创建的是无头service。

无头service一般用于有状态的应用场景，如Kaka集群、Redis集群等，这类pod之间需要相互通信相互组成集群，不在需要所谓的service负载均衡。

# deployment怎么扩容或缩容？

直接修改pod副本数即可，可以通过下面的方式来修改pod副本数：

```Go
kubectl scale --replicas=5 deployment/deployment-name
```

# deployment的更新升级策略有哪些？

deployment的升级策略主要有两种。

- Recreate 重建更新：这种更新策略会杀掉所有正在运行的pod，然后再重新创建的pod；
- rollingUpdate 滚动更新：这种更新策略，deployment会以滚动更新的方式来逐个更新pod，同时通过设置滚动更新的两个参数maxUnavailable、maxSurge来控制更新的过程。

# 简述一下deployment的更新过程?

滚动更新,每更新一次deployment，都会创建新的replicaset

# deployment的回滚使用什么命令

```Bash
kubectl rollout history deployment/deployment-nginx
kubectl rollout undo deployment/deployment-nginx --to-revision=2
```

# 存储卷分类，作用分别是什么?

- emptyDir：用于存储临时数据的简单空目录，emptyDir的数据随着容器的消亡也会销毁
- hostPath：用于将目录从工作节点的文件系统挂载到pod中

# pv的访问模式有哪几种

pv的访问模式有3种，如下：

- ReadWriteOnce，简写：RWO 表示，只仅允许单个节点以读写方式挂载；
- ReadOnlyMany，简写：ROX 表示，可以被许多节点以只读方式挂载；
- ReadWriteMany，简写：RWX 表示，可以被多个节点以读写方式挂载；

# pv的回收策略有哪几种

主要有3种回收策略：retain 保留、delete 删除、 Recycle回收。

- Retain：保留，该策略允许手动回收资源，当删除PVC时，PV仍然存在，PV被视为已释放，管理员可以手动回收卷。
- Delete：删除，如果Volume插件支持，删除PVC时会同时删除PV，动态卷默认为Delete，目前支持Delete的存储后端包括AWS EBS，GCE PD，Azure Disk，OpenStack Cinder等。
- Recycle：回收，如果Volume插件支持，Recycle策略会对卷执行rm -rf清理该PV，并使其可用于下一个新的PVC，但是本策略将来会被弃用，目前只有NFS和HostPath支持该策略。（这种策略已经被废弃，不用记）

# 在pv的生命周期中，一般有几种状态

pv的的状态有以下4种：Available（可用）、Bound（已绑定）、Released（已释放）、Failed（失败）

- Available，表示pv已经创建正常，处于可用状态；
- Bound，表示pv已经被某个pvc绑定，注意，一个pv一旦被某个pvc绑定，那么该pvc就独占该pv，其他pvc不能再与该pv绑定；
- Released，表示pvc被删除了，pv状态就会变成已释放；
- Failed，表示pv的自动回收失败；

# DaemonSet资源对象的特性？

一般作为守护进程，DaemonSet一般使用的场景有

- 在去做每个节点的日志收集工作；
- 监控每个节点的的运行状态;

# 如何停机维护又不能影响业务应用

- 先将节点设置成不可调度状态
- 驱逐节点上的pod。daemonset除外

kubectl drain 命令

# etcd集群节点可以设置为偶数个吗，为什么要设置为基数个呢？

不能，也不建议这么设置。

底层的原理，涉及到集群的脑裂 ，Raft协议leader选举问题

# etcd集群节点之间是怎么同步数据的？

通过Raft协议进行节点之间数据同步， 保证节点之间的数据一致性

# kube-proxy原理?

集群中每个Node上都会运行一个kube-proxy服务进程，他是Service的透明代理兼均衡负载器，其核心功能是将某个Service的访问转发到后端的多个Pod上。

kube-proxy通过监听集群状态变更，并对本机iptables做修改，从而实现网络路由。

从V1.8版本开始，用IPVS（IP Virtual Server）模式，用于路由规则的配置，主要优势是：

1）为大型集群提供了更好的扩展性和性能。采用哈希表的数据结构，更高效；

2）支持更复杂的负载均衡算法；

3）支持服务器健康检查和连接重试；

4）可以动态修改ipset的集合；

# 简述kube-proxy ipvs和iptables的异同?

iptables是为防火墙而设计的；IPVS则专门用于高性能负载均衡，并使用更高效的数据结构（Hash表），允许几乎无限的规模扩张。

与iptables相比，IPVS拥有以下明显优势：为大型集群提供了更好的可扩展性和性能；支持比iptables更复杂的复制均衡算法（最小负载、最少连接、加权等）；支持服务器健康检查和连接重试等功能；可以动态修改ipset的集合，即使iptables的规则正在使用这个集合；

# K8S 资源限制 QoS？

Quality of Service（Qos）

主要有三种类别：

1）BestEffort：什么都不设置（CPU or Memory），佛系申请资源；

2）Burstable：Pod 中的容器至少一个设置了CPU 或者 Memory 的请求；

3）Guaranteed：Pod 中的所有容器必须设置 CPU 和 Memory，并且 request 和 limit 值相等；

# k8s数据持久化的方式有哪些？

- EmptyDir（空目录）：
- Hostpath：将宿主机上已存在的目录或文件挂载到容器内部。
- PersistentVolume（简称PV）：

# K8s中镜像的下载策略是什么？

- Always：镜像标签为latest时，总是从指定的仓库中获取镜像；
- Never：禁止从仓库中下载镜像，也就是说只能使用本地镜像；
- IfNotPresent：仅当本地没有对应镜像时，才从目标仓库中下载；

# kubelet 监控 Node 节点资源使用是通过什么组件来实现的？

用Metrics Server提供核心指标，包括Node、Pod的CPU和内存的使用。而Metrics Server需要采集node上的cAdvisor提供的数据资源

# kubernetes服务发现？

- 环境变量： 当你创建一个Pod的时候，kubelet会在该Pod中注入集群内所有Service的相关环境变量。
- DNS：可以通过cluster add-on方式轻松的创建KubeDNS来对集群内的Service进行服务发现；

# 简述ETCD及其特点?

1）完全复制：集群中的每个节点都可以使用完整的存档；

2）高可用性：Etcd可用于避免硬件的单点故障或网络问题；

3）一致性：每次读取都会返回跨多主机的最新写入；

4）简单：包括一个定义良好、面向用户的API（gRPC）；

5）安全：实现了带有可选的客户端证书身份验证的自动化TLS；

6）快速：每秒10000次写入的基准速度；

7）可靠：使用Raft算法实现了强一致、高可用的服务存储目录；

# 简述Kubernetes初始化容器（init container）?

Init container用用于side car做一些初始化处理。

init container的运行方式与应用容器不同，它们必须先于应用容器执行完成，当设置了多个init container时，将按顺序逐个运行，并且只有前一个init container运行成功后才能运行后一个init container。

# 简述Kubernetes自动扩容HPA机制?

Kubernetes使用Horizontal Pod Autoscaler（HPA）的控制器实现基于CPU使用率进行自动Pod扩缩容的功能。

HPA控制器周期性地监测目标Pod的资源性能指标，并与HPA资源对象中的扩缩容条件进行对比，在满足条件时对Pod副本数量进行调整；

# 简述Kubernetes Service分发后端的策略?

1）RoundRobin：默认为轮询模式，即轮询将请求转发到后端的各个Pod上；

2）SessionAffinity：基于客户端IP地址进行会话保持的模式，即第1次将某个客户端发起的请求转发到后端的某个Pod上，之后从相同的客户端发起的请求都将被转发到后端相同的Pod上；

# 简述Kubernetes Scheduler作用及实现原理?

Scheduler是负责Pod调度的重要功能模块，负责接收Controller Manager创建的新Pod，为其调度至目标Node，调度完成后，目标Node上的kubelet服务进程接管后继工作，负责Pod接下来生命周期；

# 简述Kubernetes Scheduler使用哪两种算法将Pod绑定到worker节点

- 预选（Predicates）：输入是所有节点，输出是满足预选条件的节点。kube-scheduler根据预选策略过滤掉不满足策略的Nodes。如果某节点的资源不足或者不满足预选策略的条件则无法通过预选；
- 优选（Priorities）：输入是预选阶段筛选出的节点，优选会根据优先策略为通过预选的Nodes进行打分排名，选择得分最高的Node。例如，资源越富裕、负载越小的Node可能具有越高的排名；

# 简述Kubernetes准入机制?

AlwaysAdmit：允许所有请求；

AlwaysDeny：禁止所有请求，多用于测试环境；

ServiceAccount：它将serviceAccounts实现了自动化，它会辅助serviceAccount做一些事情，比如如果pod没有serviceAccount属性，它会自动添加一个default，并确保pod的serviceAccount始终存在；

LimitRanger：观察所有的请求，确保没有违反已经定义好的约束条件，这些条件定义在namespace中LimitRange对象中；

ResourceQuota

NamespaceExists：观察所有的请求，如果请求尝试创建一个不存在的namespace，则这个请求被拒绝；

# 简述Kubernetes网络模型?

Kubernetes网络模型中每个Pod都拥有一个独立的IP地址，不管它们是否运行在同一个Node（宿主机）中，都要求它们可以直接通过对方的IP进行访问；

同时为每个Pod都设置一个IP地址的模型使得同一个Pod内的不同容器会共享同一个网络命名空间，也就是同一个Linux网络协议栈。

这就意味着同一个Pod内的容器可以通过localhost来连接对方的端口；在Kubernetes的集群里，IP是以Pod为单位进行分配的。一个Pod内部的所有容器共享一个网络堆栈；

# 简述Kubernetes CNI模型?

Kubernetes CNI模型是对容器网络进行操作和配置的规范，通过插件的形式对CNI接口进行实现。

CNI仅关注在创建容器时分配网络资源，和在销毁容器时删除网络资源。

容器（Container）：是拥有独立Linux网络命名空间的环境，例如使用Docker或rkt创建的容器。容器需要拥有自己的Linux网络命名空间，这是加入网络的必要条件；

网络（Network）：表示可以互连的一组实体，这些实体拥有各自独立、唯一的IP地址，可以是容器、物理机或者其他网络设备（比如路由器）等；

# 简述Kubernetes PV和PVC?

PV是对底层网络共享存储的抽象，将共享存储定义为一种“资源”；

PVC则是用户对存储资源的一个“申请”；

# 简述Kubernetes CSI模型?

CSI是Kubernetes推出与容器对接的存储接口标准，存储提供方只需要基于标准接口进行存储插件的实现，就能使用Kubernetes的原生存储机制为容器提供存储服务，CSI使得存储提供方的代码能和Kubernetes代码彻底解耦，部署也与Kubernetes核心组件分离；

CSI包括CSI Controller：的主要功能是提供存储服务视角对存储资源和存储卷进行管理和操作；Node的主要功能是对主机（Node）上的Volume进行管理和操作；