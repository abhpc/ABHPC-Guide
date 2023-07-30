# ABHPC集群使用和管理教程

遵循一般Linux系统的规则，**凡命令行开头为"#"的，表明此时正在使用root账户，而命令行开头为"$"的，则表明此时在使用普通用户**。建议没有Linux系统使用经验的用户，首先阅读[Linux常用命令](User/Linux常用命令.md)。

本教程中，部分命令是ABHPC特有的脚本命令，在此不再详细说明。

本项目针对普通用户和管理员提供以下教程：

## 目录

* [1.普通用户](#1%E6%99%AE%E9%80%9A%E7%94%A8%E6%88%B7)
  + [1.1 使用ssh客户端连接集群](#11-%E4%BD%BF%E7%94%A8ssh%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%BF%9E%E6%8E%A5%E9%9B%86%E7%BE%A4)
  + [1.2 使用Nomachine连接图形界面](#12-%E4%BD%BF%E7%94%A8nomachine%E8%BF%9E%E6%8E%A5%E5%9B%BE%E5%BD%A2%E7%95%8C%E9%9D%A2)
  + [1.3 用户作业管理基本操作](#13-%E7%94%A8%E6%88%B7%E4%BD%9C%E4%B8%9A%E7%AE%A1%E7%90%86%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)
  + [1.4 进阶：并行计算基础知识](#14-%E8%BF%9B%E9%98%B6%E5%B9%B6%E8%A1%8C%E8%AE%A1%E7%AE%97%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86)
  + [1.5 常见错误](#15-%E5%B8%B8%E8%A7%81%E9%94%99%E8%AF%AF)
* [2.集群管理员](#2%E9%9B%86%E7%BE%A4%E7%AE%A1%E7%90%86%E5%91%98)
  + [2.1 新建集群用户](#21-%E6%96%B0%E5%BB%BA%E9%9B%86%E7%BE%A4%E7%94%A8%E6%88%B7)
  + [2.2 Slurm系统管理](#22-slurm%E7%B3%BB%E7%BB%9F%E7%AE%A1%E7%90%86)
  + [2.3 常用软件的安装和注意事项](#23-%E5%B8%B8%E7%94%A8%E8%BD%AF%E4%BB%B6%E7%9A%84%E5%AE%89%E8%A3%85%E5%92%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)

## 1.普通用户

### 1.1 使用ssh客户端连接集群

### 1.2 使用Nomachine连接图形界面

### 1.3 用户作业管理基本操作

### 1.4 进阶：并行计算基础知识

问题：核用得越多，我的计算速度越快吗？

**回答**：不一定。首先要弄清楚并行计算的基本原理：所谓并行计算，就是把一个计算任务分割成几部分，交给不同的计算单元去计算。

以LAMMPS为例，每计算一个时间步长，各核之间就要通信一次，以交换各核之间的边界原子的信息。那么在一个计算周期中的计算时间分为2部分：

$$T_{\rm tot} = T_{\rm cal} + T_{\rm com} $$

其中 $T_{\rm tot}$ 是计算所需总时间， $T_{\rm cal}$ 是单核完成计算的时间， $T_{\rm com}$ 是各核完成计算后的同步时间。

如下图所示，核数越多，各计算核心分配到的计算任务（原子数）越少，那么 $T_{\rm cal}$ 就会相应降低；另一方面，核与核之间的边界原子会随着核数的增加呈几何级数增加，通信时间也就会不断增加， $T_{\rm com}$ 会急剧增大。因此，增加计算核数的后果是， $T_{\rm cal}$ 减小， $T_{\rm com}$ 增大，总计算时间 $T_{\rm tot} = T_{\rm cal} + T_{\rm com}$ 不一定减小。只有当 $T_{\rm cal}$ 减小的部分大于 $T_{\rm com}$ 增大的部分，这样的计算才是加速的，否则计算反而会变慢。

为了减小 $T_{\rm com}$ ，建设集群需要花费大量资金购买Infiniband交换机，这是为了尽可能地降低通信的延迟，从而降低通信时间。

所以，并不是核数用得越多，计算速度越快，当核数多到一定程度的时候，计算速度反而有所下降，甚至还会引起其他的问题，比如计算卡死。

<div  align="center">    
<img src="images/并行计算常见误区1.png" width = "300" height = "300" alt="图片名称" align=center />
</div>


### 1.5 常见错误

[ssh远程连接集群](User/ssh远程连接集群.md)

[远程桌面连接集群](User/使用x2go连接Ubuntu远程桌面.md)

[Slurm用户教程](User/Slurm用户教程.md)

[常见错误](User/常见错误.md)

[并行计算常用知识](并行计算常用知识)

## 2.集群管理员

### 2.1 新建集群用户
[CentOS 7](Admin/CentOS_7/新建集群用户.md)

[Ubuntu 16.04LTS](Admin/Ubuntu_16.04/新建集群用户.md)

### 2.2 Slurm系统管理

[Slurm管理教程](Admin/Slurm管理教程.md)

### 2.3 常用软件的安装和注意事项

（1）[Gaussian 2016](Admin/高斯用户组设置.md)



[配置服务器的远程MATE桌面](Admin/配置服务器的远程MATE桌面.md)

[常用软件的安装](常用软件的安装)
