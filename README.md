# yiqiu - 奕秋（部署平台）

<!-- vim-markdown-toc GFM -->

* [项目说明](#项目说明)
* [项目参考](#项目参考)
* [参加步骤](#参加步骤)

<!-- vim-markdown-toc -->
## 项目说明

首先服务如果容器化的话，目前 k8s 是一个编排容器的工具，是管理应用的全生命周期的一个服务，从创建应用，应用的部署，应用提供服务，扩容缩容应用，应用更新，都非常的方便，而且可以做到故障自愈。非常强大。

这里【奕秋平台】仅用于管理物理部署（如服务器全为统一系统的虚拟机）范围，如果服务容器化的话可以使用 k8s

另外，此项目仅供学习和探索，主要是记录一些想法

## 项目参考

> * [剖析 AWS CodeDeploy](https://infoq.cn/article/analysis-of-aws-codedeploy)

## 参加步骤

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
