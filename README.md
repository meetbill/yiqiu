# yiqiu - 奕秋（部署平台）

<!-- vim-markdown-toc GFM -->

* [1 项目说明](#1-项目说明)
    * [1.1 项目目标](#11-项目目标)
        * [三个标准化](#三个标准化)
        * [需要支持有状态服务](#需要支持有状态服务)
* [2 项目设计](#2-项目设计)
    * [2.1 项目架构](#21-项目架构)
    * [2.2 服务模型](#22-服务模型)
        * [分层](#分层)
        * [关联关系](#关联关系)
* [3 项目组件](#3-项目组件)
* [4 参加步骤](#4-参加步骤)

<!-- vim-markdown-toc -->
## 1 项目说明

首先服务如果容器化的话，目前 k8s 是一个编排容器的工具，是管理应用的全生命周期的一个服务，从创建应用，应用的部署，应用提供服务，扩容缩容应用，应用更新，都非常的方便，而且可以做到故障自愈。非常强大。

这里【奕秋平台】仅用于管理物理部署（如服务器全为统一系统的虚拟机）范围，如果服务容器化的话可以使用 k8s

另外，此项目仅供学习和探索，主要是记录一些想法，非投产项目

### 1.1 项目目标

#### 三个标准化

![Screenshot](./images/target.png)

将服务标准化，以达到“You build it，you run it.”的目的

#### 需要支持有状态服务

一般情况下，nginx 或者 web server（不包含 MySQL) 自身都是不需要保存数据的，对于 web server, 数据会保存在专门做持久化的节点上。所以这些节点可以随意扩容或者缩容，只要简单的增加或减少副本的数量就可以。

但是很多有状态的程序都需要集群式的部署，意味着节点需要形成群组关系，每个节点需要一个唯一的 ID（例如 Kafka BrokerId, Zookeeper myid) 来作为集群内部每个成员的标识，集群内节点之间进行内部通信时需要用到这些标识。

## 2 项目设计

### 2.1 项目架构

![Screenshot](./images/arch.png)

通过平台解决各服务运维的共性问题并统一服务运维方式

### 2.2 服务模型

#### 分层

假如一个集群服务有 4 个组件，组件间有依赖关系（组件间通常是有向无环图），每个组件有多个实例

从整体上可以分为三层

> * Cluster
> * Service 组件
> * Unit 组件中的实际实例

#### 关联关系

关联关系即 service 上下游的依赖关系

一般情况下，对集群中服务的配置文件的变更主要涉及两类

> * 服务本身的配置，如 redis 是否开持久化，持久化的方式，maxmemory 的大小等等
> * 服务依赖某个下游，如果下游有实例变动，则需要更新配置文件中下游的访问地址

而这的关联关系就是服务对下游的依赖描述

将此块描述放到部署包中，依托平台达到当下游实例变更时，上游配置自动更新，以达到“You build it，you run it.”的目的

这里的上游自动更新会根据关联关系自动增加 / 删除变动的下游实例

```
ps: 讨论下
(1) 下游的实例变更时，也可以由某个中控节点自动更新上游的配置，
    缺点：此种方式集群服务的组件需要有中控模块，下游实例需要向中控节点进行注册等操作，依赖中控模块程度高
    优点：当下游服务为有状态服务时，对上游服务的配置进行更新时容易定制
(2) 上游访问下游组件时也可以是个 VIP，如 k8s 中的 Service（定义了外界访问一组 Pod 的方式）
```

## 3 项目组件

> * [Pine agent](https://github.com/meetbill/butterfly) 用于管理 program,program 是服务标准化的抽象

## 4 参加步骤

* 在 GitHub 上 `fork` 到自己的仓库，然后 `clone` 到本地，并设置用户信息。
```
$ git clone https://github.com/meetbill/yiqiu.git
$ cd yiqiu
$ git config user.name "yourname"
$ git config user.email "your email"
```
* 修改代码后提交，并推送到自己的仓库。
```
$ #do some change on the content
$ git commit -am "Fix issue #1: change helo to hello"
$ git push
```
* 在 GitHub 网站上提交 pull request。
* 定期使用项目仓库内容更新自己仓库内容。
```
$ git remote add upstream https://github.com/meetbill/yiqiu.git
$ git fetch upstream
$ git checkout master
$ git rebase upstream/master
$ git push -f origin master
```
