---
title: PaaS网络模型设计
date: 2017-07-12 17:18:01
categories: "开发"
tags:
	- Nginx
	- Docker
	- IaaS
	- OpenStack
	- 云计算

---

【摘要】PaaS的抽象逻辑使得其网络模型与IaaS有所不通，本次通过重点说明Kubernetes和Docker的网络，从而来探究PaaS的网络模型设计。

PaaS和IaaS的网络需求

概念上来说，IaaS是基础架构即服务，PaaS是平台即服务。从服务化角度来说两者对于网络的需求有共同点：

1.  一台宿主机的网络隔离，一台宿主机要运行多个环境黑盒（VM或者容器），这时候每个环境黑盒的网络需要隔离。
2.  环境变更后的访问一致性，比如VM或者容器迁移到其他宿主机的时候，如何保证外部访问不感知，比较通用的解决方案网络代理层来解决。

实现上来说，IaaS是VM管理，PaaS是容器编排，两者的网络也会有些不同：

1.  容器比VM轻量级，启停更快，更方便迁移，所以PaaS的整个调度策略会更灵活，容器的迁移频率是高于VM，当容器迁移的时候，PaaS需要更加快速的解决变化后的网络访问。
2.  VM安全高于容器，IaaS这部分会更多在隔离和安全上有所考虑，当然这个可能是公有云和私有云的一个定位，个人认为IaaS比较适合公有云，PaaS比较适合私有云。

PaaS的网络模型设计

以上部分我们讨论了PaaS网络需求，总结来说PaaS需要解决宿主机的网络隔离和环境变更后的访问一致性问题，然后在灵活性上需要更加注重，私有云的定位可以减少因为安全和隔离的代价，保证高性能。

宿主机的网络隔离

网络隔离最基本问题就是要解决端口冲突，一种思路是容器通过端口映射访问，宿主机的端口由系统分配避免端口冲突，这种方式对使用的便利性是有意义的，但并不理想，因为需要对各种端口进行映射，这会限制宿主机的能力，在容器编排上也增加了复杂度。

端口是个稀缺资源，这就需要解决端口冲突和动态分配端口问题。这不但使调度复杂化，而且应用程序的配置也将变得复杂，具体表现为端口冲突、重用和耗尽。

NAT将地址空间分段的做法引入了额外的复杂度。比如容器中应用所见的IP并不是对外暴露的IP，因为网络隔离，容器中的应用实际上只能检测到容器的IP，但是需要对外宣称的则是宿主机的IP，这种信息的不对称将带来诸如破坏自注册机制等问题。

所以就要引入一层网络虚拟层来解决网络隔离，现在说的比较多的大二层网络，L2 over L3，比如OVS、Flannel、Calico等等。

环境变更后的访问一致性

一个通用方案来说通过代理层提供不变的访问入口，像OpenStack的网络节点就是一个L3（IP）的访问入口，而PaaS更多的是提供L4（TCP、UDP）和L7（HTTP）的访问，L4比较流行的开源方案有LVS，L7的是Nginx和Haproxy。

因此PaaS的网络结构有：

1.  物理网络
2.  虚拟层网络
3.  代理层网络

Kubernetes和Docker的网络说明

Kubernete Docker作为目前最流行的开源PaaS实现，通过其实现细节可以更加理解PaaS的网络模型实践。

容器网络

Docker使用Linux桥接，在宿主机虚拟一个Docker容器网桥（docker0），Docker启动一个容器时会根据Docker网桥的网段分配给容器一个IP地址，称为Container-IP，同时Docker网桥是每个容器的默认网关。因为在同一宿主机内的容器都接入同一个网桥，这样容器之间就能够通过容器的Container-IP直接通信。

![PaaS网络模型设计][PaaS]

Docker网桥是宿主机虚拟出来的，并不是真实存在的网络设备，外部网络是无法寻址到的，这也意味着外部网络无法通过直接Container-IP访问到容器。

Pod内部容器通信

Kubernetes中Pod是容器的集合，Pod包含的容器都运行在同一个宿主机上，这些容器将拥有同样的网络空间，容器之间能够互相通信，它们能够在本地访问其他容器的端口。

实际上Pod都包含一个网络容器，它不做任何事情，只是用来接管Pod的网络，业务容器通过加入网络容器的网络从而实现网络共享。

Pod的启动方式类似于：

    $ docker run -p 80:80 --name network-container -d <network-container-image>

所以Pod的网络实际上就是Pod中网络容器的网络，所以往往就可以认为Pod网络就是容器网络，在理解Kubernetes网络的时候这是必须要需要清楚的，比如说Pod的Pod-IP就是网络容器的Container-IP。

![PaaS网络模型设计][PaaS 1]

Pod间通信

Kubernetes网络模型是一个扁平化网络平面，在这个网络平面内，Pod作为一个网络单元同Kubernetes Node的网络处于同一层级。

![PaaS网络模型设计][PaaS 2]

在这个网络拓扑中满足以下条件：

 *  Pod间通信：Pod1和Pod2（同主机），Pod1和Pod3（跨主机）能够通信
 *  Node与Pod间通信：Node1和Pod1/ Pod2（同主机），Pod3（跨主机）能够通信

为此需要实现：

 *  Pod的Pod-IP是全局唯一的。其实做法也很简单，因为Pod的Pod-IP是Docker网桥分配的，所以将不同Kubernetes Node的Docker网桥配置成不同的IP网段即可。
 *  Node上的Pod/容器原生能通信，但是Node之间的Pod/容器如何通信的，这就需要对Docker进行增强，在容器集群中创建一个覆盖网络（Overlay Network），联通各个节点，目前可以通过第三方网络插件来创建覆盖网络，比如Flannel和Open vSwitch等等。

Flannel由CoreOS团队设计开发的一个覆盖网络工具，Flannel 通过在集群中创建一个覆盖网络，为主机设定一个子网，通过隧道协议封装容器之间的通信报文，实现容器的跨主机通信。Flannel将会运行在所有的Node之上，Flannel实现的网络结构如图所示：

![PaaS网络模型设计][PaaS 3]

代理层

Kubernetes中的Service就是在Pod之间起到服务代理的作用，对外表现为一个单一访问接口，将请求转发给Pod，Service的网络转发是Kubernetes实现服务编排的关键一环。

Service都会生成一个虚拟IP，称为Service-IP， Kuberenetes Porxy组件负责实现Service-IP路由和转发，在容器覆盖网络之上又实现了虚拟转发网络。

Kubernetes Porxy实现了以下功能：

 *  转发访问Service的Service-IP的请求到Endpoints（即Pod-IP）。
 *  监控Service和Endpoints的变化，实时刷新转发规则。
 *  负载均衡能力。

Kubernetes Porxy是一种分布式L3代理转发， 默认是基于Iptables实现，这从设计上很值得借鉴，但是性能部分有待验证。

Kubernetes中的Ingress提供了一个统一的对外代理入口配置，比如HTTP的访问配置，然后通过实现Ingress-Controller，Ingress-Controller的实现可以用Nginx、LVS等等，以Nginx来说，就Ingress-Controller监控Kubernetes API，生成Nginx配置，然后加载生效，而Nginx跟Pod的通信，可以走Service，但是考虑到Kubernetes Porxy的性能问题，建议直接和Pod通信。下面就是一个实现图：

![PaaS网络模型设计][PaaS 4]

整体来看的一个网络模型如下：

![PaaS网络模型设计][PaaS 5]

Q&A

Q：有这么多虚拟网络，覆盖网络，会不会有网络延迟？

> A：网络虚拟会带来性能损耗，比如Flannel需要将报文封装到UDP包中传输，这中间的封包解包就会带来损耗。所以网络虚拟化的部分，软件的实现还有待优化，其实更好的方式是硬件支持，比如现在提的很多的SDN网络。

Q：Pod为什么要有个网络容器？

> A： 一方面这是解耦，通过网络容器来负责网络配置，这样对于业务容器来说稳定性会更高，比如在多个业务容器中，某一个业务容器断了，这样就不会导致网络中断。

Q：Calico默认全网是打通的，怎么做基于网段之间的隔离？

> A：目前来说要做网段隔离，可能偏向安全性比较高的场景，我们目前是做私有云场景，对隔离的要求没那么高。所以如果要做隔离的话，可以参考OpenStack的OVS方案。

Q：在某些应用场景下，Pod的IP需要固定，而不是重启之后IP可能会变化，请问如何满足这种场景的需求？

> A：Pod的IP需要固定的话，一种方式是修改Docker的代码，据我所知腾讯有实现过。另外的方案就是做L3的代理，给Pod一个浮动IP，有点像OpenStack的实现。

Q：Ingress的流量默认是先走Service然后到Pod，还是直接到Pod？

> A：取决你的实现，官方的实现是先走Sevice再到Pod，我们是直接到Pod。

Q：Ingress-Controller实现除了使用LVS和Nginx外，能否采用商用负载设备来实现？实现是否取决于和Kubernetes API的对接？

> A：可以，只要有接口都可以实现，通过实现Ingress-Controller，比如对接F5的硬件设备，只要F5支持相关的API。

Q：代理入口上有哪些方法解决单点失效的问题呢？

> A：这个比较传统了，软件实现就Keepalived之类的。

Q：Igress-Cntroller比较好的库都有哪些，分别基于Nginx Haproxy LVS的？

> A：GitHub有蛮多实现的，其实还是比较简单的，像go语言的话，直接套用官方提供的demo即可，其他语言可以对接Kubernetes的API实现。

Q：这么多层的网络，多层转发后网络性能怎么样？有没有办法实现高速数据转发解决方案？

> A：代理层，虚拟层都会有损耗，这个就是要考虑管理成本和性能的权衡了。如何要实现高性能的话，就是要往SDN网络研究，通过硬件层的支持来实现虚。


[PaaS]: /pro/os/crawler/URRB-E3UA-2QBB.jpg
[PaaS 1]: /pro/os/crawler/E7NN-EFA3-MFEI.jpg
[PaaS 2]: /pro/os/crawler/NZ22-U3RM-2MN2.jpg
[PaaS 3]: /pro/os/crawler/F2UM-IQEB-6RIQ.jpg
[PaaS 4]: /pro/os/crawler/FIRM-QEBV-73IB.jpg
[PaaS 5]: /pro/os/crawler/ZFUE-FAQF-YY2E.jpg
 *  **原文作者：** 零空科技
 *  **原文链接：** https://www.toutiao.com/item/6441811343803679245/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。