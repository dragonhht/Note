---
title: ContinuousIntegration.md
date: 2018-11-21 10:56:52
tags: 
categories: 
---

**目录 start**
 
1. [持续集成](#持续集成)
    1. [Jenkins](#jenkins)
    1. [GoCD](#gocd)
    1. [Drone](#drone)
    1. [flow.ci](#flowci)
    1. [三方平台](#三方平台)
1. [代码质量管理](#代码质量管理)
    1. [sonarqube](#sonarqube)
        1. [小型项目目前使用的方案](#小型项目目前使用的方案)

**目录 end**|_2019-04-19 13:05_| [Kuangcp](https://github.com/Kuangcp/Note) | [yi-yun](https://github.com/yi-yun/Memo)
****************************************
# 持续集成
> 参考博客: [持续集成](http://www.ruanyifeng.com/blog/2015/09/continuous-integration.html) | [持续集成服务 Travis CI 教程](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)  
> [廖雪峰 使用Travis进行持续集成](https://www.liaoxuefeng.com/article/0014631488240837e3633d3d180476cb684ba7c10fda6f6000)  
> 目前个人理解: 使用jenkins 结合gradle docker ，一键上传代码之后自动构建得到镜像

> [利用Travis CI更新github page](https://github.com/steveklabnik/automatically_update_github_pages_with_travis_example)
- 使用bitbucket配置私有仓库，在hub上配置docker文件的目录，进行构建，这样就会得到一个可用的镜像
    - 源码是过去了，构建呢，这是个问题，可以使用Jenkins么？

**************************
## Jenkins
> [详细](Jenkins.md)

## GoCD
> [Github:GoCD](https://github.com/GoCD) 

> [参考博客: GoCD的正确打开方式](https://insights.thoughtworks.cn/the-right-interpretation-of-gocd/)

> [参考博客: GoCD概念篇](http://www.cnblogs.com/elisun/p/7071536.html)
************************
## Drone 
> [官网](https://drone.io/)

一个原生支持 docker 的 CI

> [参考博客: Drone 一个原生支持 docker 的 CI](https://aisensiy.github.io/2017/08/04/drone-best-ci/)  
> [参考博客: Drone CI + GitLab持续集成的基础设施搭建](https://zmcdbp.com/drone-ci-gitlab-base-build/) | [参考博客: Drone CI的持续集成的基本使用](https://zmcdbp.com/drone-ci-basic-use/)

*******************
## flow.ci
> [官网](https://flow.ci/) | [文档](https://github.com/FlowCI/docs/blob/master/intro_base.md)


## 三方平台
- [appveyor](https://ci.appveyor.com/projects)

> [Gradle + Travis CI 学习笔记](https://upupming.site/2018/04/03/gradle-travis/#travis-ci)  

****************************
# 代码质量管理

## sonarqube
> [官网](https://www.sonarqube.org/)

### 小型项目目前使用的方案
- 在开发机上进行开发，然后使用脚本将war上传scp到指定文件夹下，然后执行docker命令进行构建镜像，然后运行容器
