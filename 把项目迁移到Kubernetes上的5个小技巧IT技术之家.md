---
title: 把项目迁移到Kubernetes上的5个小技巧
date: 2018-05-06 12:09:00
categories: "开发"
tags:
	- Docker
	- Linux
	- 软件
	- Go语言
	- YAML

---

我们将在本文中提供5个诀窍帮你将项目迁移到Kubernetes上，这些诀窍来源于过去12个月中OpenFaas社区的经验。下文的内容与Kubernetes 1.8兼容，并且已经应用于OpenFaaS - Serverless Functions Made Simple的实践中。

免责声明

> 因为Kubernetes 的API更新的特别频繁，请参�����官方文档获得最新信息。

1. 将所有的内容都放进Docker

第一步操作是给以独立进程方式运行的每个组件创建一个Dockerfile，这看起来是显而易见的。如果你已经这么做了，那么你已经快人一步了。

但是如果你还没有开始这么做，那么请确保你的每一个组件都在使用多阶段构建。一个多阶段的构建要用到两个Docker镜像： 一个是构建时；一个是运行时。举例来说，基础的镜���可���是一个Go SDK用来编译二进制文件，最后阶段则是一个类似Alpine Linux的最小的Linux镜像。我们将二进制文件拷贝到最终阶段的镜像中，安装类似CA证书这样的软件包，然后设置入口（entry-point）。这样你最后得到的镜像体积很小而且不会包括不需要的软件包。

这里给出一个例子： Go写的OpenFaaS API gateway 组件的多阶段构建。你会注意到它里面包含的一些其它实践：

 *  使���一个非root用户的运行时
 *  将构建时的阶段命名为类似`build`
 *  指定构建的基础架构，比如`linux`
 *  使用版本做标签，比如`3.6`。 如果你使用`latest`，那会导致不可预知的情况。

例子如下：

``````````
FROM golang:1.9.4 as buildWORKDIR /go/src/github.com/openfaas/faas/gatewayCOPY . .RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o gateway .FROM alpine:3.6RUN addgroup -S app && adduser -S -g app appWORKDIR /home/appEXPOSE 8080ENV http_proxy ""ENV https_proxy ""COPY --from=build /go/src/github.com/openfaas/faas/gateway/gateway .COPY assets assetsRUN chown -R app:app ./USER appCMD ["./gateway"]
``````````

注意：

> 如果你想要使用OpenShift（一个Kubernetes的发行版），那么你必须保证你所有的Docker镜像都是以非root用户运行的。

1.1 获得Kubernetes

你需要在你的笔记本或者开发机上装好Kubernetes。 可以阅读我写的一篇博文，描述了在Mac上运行Docker和Kubernetes的所有常用选项。

如果你之前已经用过Docker了，那么你可能已经熟悉容器这个词了。在Kubernetes的词汇表里面你会很少直接操作容器，取而代之的是抽象为Pod的概念。

一个Pod是一个到多个容器组成的一个组，里面的这些容器被集中的调度和部署并通过环回接口127.0.0.1互相访问。

这里给出一个例子，说明Pod抽象的用处：比方说你有一个传统的应用，本身没有TLS/SSL支持。它可以与一个配置了TLS的Nginx或者其它Web服务器部署到一个Pod中。这么做的优点是可以将多个容器部署到一起来扩展它的功能而不会带来破坏性改变。

2. 创建YAML文件

在你有了Dockerfile和镜像之后，下一步你就需要开始写Kubernetes格式的YAML文件了。集群会读取这些文件来部署应用然后维持你项目的状态。

这与Docker本身的Compose files是不同的，刚开始你可能会觉得困难。我的���议是到���档里面找一些例子或者其它的项目试着模仿跟随它们的样式和方法。好消息就是随着经验增长你会觉得越来越容易。

每一个Docker镜像都需要在Deployment对象里面定义，指定需要运行的容器和它需要的资源。一个Deployment会创建和维持Pod来运行你的代码，如果Pod已经存在它会为你重起。

如果你想要通过HTTP/TCP来访问，那么需要为每一个组件创建一个Service对象。

你���以将多个Kubernetes定义写到一个文件里面，然后资源之间通过`---`和一个新行来分隔。但是更加普遍的做法是将定义写到多个文件里面去，每个文件代表集群中的一个API对象。

例如：

 *  gateway-svc.yml //代表一个`service服务`
 *  gateway-dep //代表一个`deployment`

如果所有的文件都在一个目录下，那么你可以通过一条命令来应用它们所有文件：

``````````
kubectl apply -f ./yaml/
``````````

当��需要运行在��它的操作系统或者架构（类似Raspberry Pi）时，我们推荐将文件放到一个新的目录里面，类似`yaml_arm`的目录名。

Deployment的例子

这里给出一个Deployment的例子，用来部署 NATS Streaming（一个轻量级的分发工作的流平台）：

``````````
apiVersion: apps/v1beta1kind: Deploymentmetadata:name: natsnamespace: openfaasspec:replicas: 1template:metadata: labels: app: natsspec: containers: - name: nats image: nats-streaming:0.6.0 imagePullPolicy: Always ports: - containerPort: 4222 protocol: TCP - containerPort: 8222 protocol: TCP command: ["/nats-streaming-server"] args: - --store - memory - --cluster_id - faas-cluster
``````````

一个`deployment`也可以声明在启动时给`service（服务）`创建多个副本或者实例。

Service 定义

``````````
apiVersion: v1kind: Servicemetadata:name: natsnamespace: openfaaslabels:app: natsspec:type: ClusterIPports:- port: 4222 protocol: TCP targetPort: 4222selector:app: nats
``````````

Service提供了一种机制可以在你的Deployment的多个副本之间对请求做负载均衡。在之前的例子中我们只有单副本的NATS Streaming，但是如果我们有多个副本，它们每个都有独立的IP地址，追踪它们就会变成问题。使用Service的优点是它可以有一个静态IP地址和DNS入口，通过它们可以随时访问到任意一个副本。

Service不是直接映射到Deployment的，它映射到label（标签）���。在上面的例���中Service会寻找`app=nats`的标签。标签可以在运行时状态下从Deployment（或者其它API对象）上增加或者删除，这样在你的集群中重定向流量就相当容易。这些可以方便的启用A/B测试或者滚动发布。

学习Kubernetes相关YAML语法的最好方式是查看官方文档里面相关的API对象的章节，你可以从中找到YAML或者`kubectl`使用的例子。

更多API对象的文档请查看：https://kubernetes.io/docs/concepts/。

2.1 Helm

Helm说它自己是Kubernetes的包管理器。 从我的观点来看它主要提供了两个主要功能：

分发你的应用（在一个Chart里面）

一旦你已经准备好分发你项目的yaml文件时，你可以将它们打包提交到Helm仓库中。这样其它人就可以找到你的应用，通过一条命令就可以安装。Chart本身可以有版本控制，也可以指定依赖与其它的Chart。

这里有三个Chart的例子：OpenFaaS、Kakfa和Minio。

让编辑���简单

Helm支持Go语言的内嵌模板，你可以将通用的配置项目放到一个文件里面。所以如果你发布了一组新的Docker镜像需要做更新，你只需要在一个地方做修改。你也可以写条件判断语句，这样将flag和`helm`命令一起使用可以在部署时启用不同的配置项和feature。

在正常的Yaml文件里面我们这样定义容器镜像：

``````````
image: functions/gateway:0.7.5
``````````

使用Helm模板我们这样做：

``````````
image: {{ .Values.images.gateway }}
``````````

然后在一个单独的文件中我们可以定义`imags.gateway`的值。Helm能让我们做的另一件事情是使用条件判断——当要支持多个架构或者feature时非常有用。

下面再给一个例子展现如何选择应用ClusterIP或者NodePort，这是暴露集群中某个服务的两种不同方式。NodePort会将服务暴露到集群以外，所以你可能需要控制什么时候���这个功能。

如果我们��用常规的YAML文件，那意味着我们需要两组配置文件：

``````````
spec:type: {{ .Values.serviceType }}ports:- port: 8080 protocol: TCP targetPort: 8080 {{- if contains "NodePort" .Values.serviceType }} nodePort: 31112 {{- end }}
``````````

在这个例子里面“.serviceType”可以是`ClusterIP`或者`NodePort`，下面的语句说明在条件满足时将`nodePort`元素加入到YAML中。

3. 使用ConfigMaps

在Kubernetes中你可以��过ConfigMap将配置文件加载��集群中。ConfigMap比“bind mounting”的方式要好因为配置文件的数据会被复制到整个集群中，保证了鲁棒性。如果数据是通过bind mount方式从一台主机上挂载的，那么你必须要事先把数据放置到这台主机中，并且同步好。这两种方式都要比把配置文件直接打进镜像的方式好，因为那样更新配置文件很不方便。

一个ConfigMap可以通过`kubectl`或者YAML文件按��调用。一旦集群中建好了一��ConfigMap，那么它就可以被添加进容器或者Pod中。

下面是一个为Prometheus定义的ConfigMap的例子：

``````````
kind: ConfigMapapiVersion: v1metadata:labels:app: prometheusname: prometheus-confignamespace: openfaasdata:prometheus.yml: |scrape_configs: - job_name: 'prometheus' scrape_interval: 5s static_configs: - targets: ['localhost:9090']
``````````

你可以将它加载进一个Deployment或者Pod中：

``````````
volumeMounts: - mountPath: /etc/prometheus/prometheus.yml name: prometheus-config subPath: prometheus.yml volumes: - name: prometheus-config configMap: name: prometheus-config items: - key: prometheus.yml path: prometheus.yml mode: 0644
``````````

查看完整例子：ConfigMap Prometheus config。

更多文档查看：https://kubernetes.io/docs/tas ... gmap/。

4. 使用安全的Secret

为了保证你密码，API Key，token等的私密性和安全性，你需要使用Kubernetes的secret管理机制���

如果你已经熟悉了ConfigMaps的���用，那么有一个好消息，secret的使用方式基本一样：

 *  在集群中定义secret
 *  通过mount加载进一个Deployment/Pod中

当你需要从一个私有的Docker镜像仓库拉镜像下来时，你可能会用到的其它的secret类型。这被称之为ImagePullSecret， 更多信息参见这里。

关于如何创建和管理secret在官方文档里面有更多信息：https://kubernetes.io/docs/con ... cret/。

5. 实���健康检查health-checks

Kubernetes通过liveness和readiness的检查来实现健康检查。我们需要利用这些机制来确保我们集群的自愈和失效保护。它们的工作方式：通过一个`探针(probe)`来在Pod里面执行一个命令或者调用一个预先定义好的HTTP入口。

Liveness

一个liveness检查可以查看程序是否在运行。针对OpenFaaS functions我们会在function启动时创建一个`/tmp/.lock`的锁文件。如果我们发现非健康状���，我们会将这个文件删除，Kubernetes就会给我们重新调度这个function。

另外一种常用方式是增加一个新的HTTP路由类似`/_/healthz`。使用`/_/`是一种传统做法因为这样不会跟已经存在的其它路由冲突。

Readiness

如果你在Kubernetes里面启用了readiness检查，那么它只会给通过测试条件的容器转发流量。

readiness检查可以是定期的执行，这与health-check不同。一个容器即使在高负载下也可以是健康的——只是这种情况我们定义为“Not ready"这样Kubernetes就会停止给它转发流量直到它恢复。

官方文档有更多关于这方面的信息：https://kubernetes.io/docs/tas ... obes/。

总结

在这篇文章中，我们列出了一些当要把项目迁移到Kubernetes上要做的一些核心工作。这包括：

 *  创建好的Docker镜像
 *  书写好的Kubernetes清单（YAML文件）
 *  使用ConfigMap来将配置和代码解耦
 *  使用Secret来��护API Key这样的隐私数据
 *  使用liveness 和readiness探针来实现弹性和自愈
 *  **原文作者：** IT技术之家
 *  **原文链接：** https://www.toutiao.com/item/6552114320925262344/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。