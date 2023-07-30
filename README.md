# ABHPC集群使用和管理教程

遵循一般Linux系统的规则，**凡命令行开头为"#"的，表明此时正在使用root账户，而命令行开头为"$"的，则表明此时在使用普通用户**。建议没有Linux系统使用经验的用户，首先阅读[Linux常用命令](User/Linux常用命令.md)。

本教程中，部分命令是ABHPC特有的脚本命令，在此不再详细说明。

本项目针对普通用户和管理员提供以下教程：

## 目录

* [1.普通用户](#1%E6%99%AE%E9%80%9A%E7%94%A8%E6%88%B7)
  + [1.1 使用ssh客户端连接集群](#11-%E4%BD%BF%E7%94%A8ssh%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%BF%9E%E6%8E%A5%E9%9B%86%E7%BE%A4)
  + [1.2 使用Nomachine连接图形界面](#12-%E4%BD%BF%E7%94%A8nomachine%E8%BF%9E%E6%8E%A5%E5%9B%BE%E5%BD%A2%E7%95%8C%E9%9D%A2)
  + [1.3 用户计算作业管理基本操作](#13-%E7%94%A8%E6%88%B7%E8%AE%A1%E7%AE%97%E4%BD%9C%E4%B8%9A%E7%AE%A1%E7%90%86%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)
    - [1.3.1 提交作业](#131-%E6%8F%90%E4%BA%A4%E4%BD%9C%E4%B8%9A)
    - [1.3.2 查看作业](#132-%E6%9F%A5%E7%9C%8B%E4%BD%9C%E4%B8%9A)
    - [1.3.3 中断作业](#133-%E4%B8%AD%E6%96%AD%E4%BD%9C%E4%B8%9A)
    - [1.3.4 查看节点情况](#134-%E6%9F%A5%E7%9C%8B%E8%8A%82%E7%82%B9%E6%83%85%E5%86%B5)
    - [1.3.5 查看队列(Partition)状态](#135-%E6%9F%A5%E7%9C%8B%E9%98%9F%E5%88%97partition%E7%8A%B6%E6%80%81)
    - [1.3.6 历史作业信息与统计](#136-%E5%8E%86%E5%8F%B2%E4%BD%9C%E4%B8%9A%E4%BF%A1%E6%81%AF%E4%B8%8E%E7%BB%9F%E8%AE%A1)
  + [1.4 进阶：并行计算基础知识](#14-%E8%BF%9B%E9%98%B6%E5%B9%B6%E8%A1%8C%E8%AE%A1%E7%AE%97%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86)
  + [1.5 常见错误](#15-%E5%B8%B8%E8%A7%81%E9%94%99%E8%AF%AF)
    - [1.5.1 error: Unable to allocate resources: Invalid account or account/partition combination specified](#151-error-unable-to-allocate-resources-invalid-account-or-accountpartition-combination-specified)
* [2.集群管理员](#2%E9%9B%86%E7%BE%A4%E7%AE%A1%E7%90%86%E5%91%98)
  + [2.1 新建集群用户](#21-%E6%96%B0%E5%BB%BA%E9%9B%86%E7%BE%A4%E7%94%A8%E6%88%B7)
  + [2.2 Slurm系统管理](#22-slurm%E7%B3%BB%E7%BB%9F%E7%AE%A1%E7%90%86)
  + [2.3 常用软件的安装和注意事项](#23-%E5%B8%B8%E7%94%A8%E8%BD%AF%E4%BB%B6%E7%9A%84%E5%AE%89%E8%A3%85%E5%92%8C%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)

## 1.普通用户

### 1.1 使用ssh客户端连接集群

### 1.2 使用Nomachine连接图形界面

### 1.3 用户计算作业管理基本操作

ABHPC使用Slurm系统进行计算作业管理。用户不必关注每个节点的情况，只需要告诉系统自己要运行什么程序、使用多少计算资源，Slurm系统即可自动对作业进行排队管理。以下是普通用户的常用操作介绍。

#### 1.3.1 提交作业

```
$ sbatch [作业脚本].slm
```
常用的slurm脚本可以在[这里下载](常用slurm脚本)。

#### 1.3.2 查看作业
```
$ squeue
```
通过此命令可以获得作业的信息如ID和运行状态等。

#### 1.3.3 中断作业
```
$ scancel [作业ID]
```
中断本用户的全部作业
```
$ scancel -u $USER
```

#### 1.3.4 查看节点情况

以测试集群为例，使用```slhosts```命令可以查看集群全部计算节点的状态。

```
$ slhosts
HOSTNAME STATE      CPUS    CPUS(A/I/O/T) FREE_MEM   REASON GRES
C01      idle       20          0/20/0/20    52609     none gpu:rtx2080:2
C02      idle       20          0/20/0/20    53734     none gpu:rtx2080:2
C03      idle       20          0/20/0/20    53692     none gpu:rtx2080:2
C04      idle       20          0/20/0/20    53740     none gpu:rtx2080:2
```
其中，CPUS(A/I/O/T)分别代表CPU的Allocated/Idle/Other/Total个数。这样可以判断哪些节点是可用
的，最大能提交多少核、多少节点。

#### 1.3.5 查看队列(Partition)状态

一般地，我们在环境变量中设置```sinfo```命令的输出格式为：
```
export SINFO_FORMAT="%10P %.6a %.6D  %.4c  %8t %16G %N"
```
其输出格式为：
```
# sinfo
PARTITION   AVAIL  NODES  CPUS  STATE    GRES             NODELIST
MX*            up      8    48  down*    (null)           A[13-20]
MX*            up     11    48  idle     (null)           A[02-12]
MXQS           up      1    48  idle     (null)           A01
```
这样可以显示多少节点可用，每个节点多少核数。


#### 1.3.6 历史作业信息与统计

ABHPC提供额外的命令```slhist```以显示历史作业，默认是显示当日的作业信息，例如：
```
$ slhist
 JobID    JobName         NodeList      User        Elapsed      State                                                      WorkDir
------ ---------- ---------------- --------- -------------- ----------        -----------------------------------------------------
     6       test    None assigned       liq       00:00:00 CANCELLED+                                               /home/liq/text
     7       test    None assigned       liq       00:00:00 CANCELLED+                                               /home/liq/text
     9       test              A01       liq       01:13:03 CANCELLED+                                               /home/liq/text
    10       test              A02       liq       00:43:00 CANCELLED+                                           /home/liq/text2/64
    11       test         A[01,03]       liq       00:42:30 CANCELLED+                                         /home/liq/text2/3232
    12       test              A01       liq       00:08:36 COMPLETED                                            /home/liq/text/scf
    13       test              A02       liq       00:05:34 COMPLETED                                        /home/liq/text2/64/scf
    14       test         A[01,03]       liq       00:10:47 COMPLETED                                      /home/liq/text2/3232/scf
    15       test              A01       liq       00:02:45 CANCELLED+                                               /home/liq/text
    16       test              A02       liq       00:26:16 RUNNING                                              /home/liq/text2/64
    17       test         A[01,03]       liq       00:24:27 RUNNING                                            /home/liq/text2/3232
```
第一列JobID是作业ID，第二列JobName是作业名，第三列Nodelist是运行的计算节点列表，第四列User是用户名，第五列Elapsed是运行时间，第六列State是作业状态，最后一列WorkDir是作业运行的目录。

也可以通过```-S  [开始时间]```和```-E   [结束时间]```参数来查看指定时间范围内的作业情况。

Slurm的时间格式为：[YYYY]-[MM]-[DD]T[hh]:[mm]:[ss]，如2019年1月1日0时0分0秒的格式写作：
```
2019-01-01T00:00:00
```


查看用户自2019年1月1日至今的作业情况：

    $ sacct -S 2019-01-01T00:00:00 -o "jobid,partition,account,user,alloccpus,cputimeraw,state,workdir%60" -X
           JobID  Partition    Account      User  AllocCPUS CPUTimeRAW      State                                                      WorkDir
    ------------ ---------- ---------- --------- ---------- ---------- ----------        -----------------------------------------------------
    52            E5-2640V4 tensorflow      lily         80         80  COMPLETED                                          /home/lily/fds-test
    53            E5-2640V4 tensorflow      lily         80          0  COMPLETED                                          /home/lily/fds-test
    54            E5-2640V4 tensorflow      lily         80          0  COMPLETED                                          /home/lily/fds-test
    55            E5-2640V4 tensorflow      lily         80          0  COMPLETED                                          /home/lily/fds-test
    56            E5-2640V4 tensorflow      lily         80          0  COMPLETED                                          /home/lily/fds-test
    57            E5-2640V4 tensorflow      lily          4          0  COMPLETED                                          /home/lily/fds-test
    58            E5-2640V4 tensorflow      lily        160          0 CANCELLED+                                          /home/lily/fds-test
    59            E5-2640V4 tensorflow      lily         80          0  COMPLETED                                          /home/lily/fds-test
    60            E5-2640V4 tensorflow      lily         80          0  COMPLETED                                          /home/lily/fds-test
    61            E5-2640V4 tensorflow      lily         80          0     FAILED                                          /home/lily/fds-test
    62            E5-2640V4 tensorflow      lily         80       2640 CANCELLED+                                          /home/lily/fds-test
    63            E5-2640V4 tensorflow      lily         80       5360 CANCELLED+                                          /home/lily/fds-test
    64            E5-2640V4 tensorflow      lily         80       7680  CANCELLED                                          /home/lily/fds-test
    65            E5-2640V4 tensorflow      lily         80      18480  CANCELLED                                          /home/lily/fds-test
    66            E5-2640V4 tensorflow      lily         80       3040  CANCELLED                                          /home/lily/fds-test
    67            E5-2640V4 tensorflow      lily         80       7920 CANCELLED+                                          /home/lily/fds-test
    68            E5-2640V4 tensorflow      lily         80      10960 CANCELLED+                                          /home/lily/fds-test
    69            E5-2640V4 tensorflow      lily         80       5040 CANCELLED+                                          /home/lily/fds-test
    70            E5-2640V4 tensorflow      lily         80        800  COMPLETED                                          /home/lily/fds-test
    71            E5-2640V4 tensorflow      lily         80        880  COMPLETED                                          /home/lily/fds-test
    72            E5-2640V4 tensorflow      lily         80        800  COMPLETED                                          /home/lily/fds-test
    73            E5-2640V4 tensorflow      lily         80      19760 CANCELLED+                                          /home/lily/fds-test
    74            E5-2640V4 tensorflow      lily         80          0 CANCELLED+                                         /home/lily/fds-test2

以上各列分别对应作业ID，队列，账户，用户，CPU开销，机时(单位秒)，状态，工作路径。

如果要统计自己使用的机时，则可以使用如下命令(也即使用awk将第6列的cputimeraw加起来)：

    $ sacct -S 2019-01-01T00:00:00 -o "jobid,partition,account,user,alloccpus,cputimeraw,state,workdir%60" -X |awk 'BEGIN{total=0}{total+=$6}END{print total}'

注意这里的输出单位是秒，换算成机时还需要除以3600。

除了直接使用sacct命令，还可以使用slhist命令。例如用户lily执行以下命令的输出为：

    $ slhist -S 2019-01-01T00:00:00
           JobID  Partition    Account      User  AllocCPUS  AllocGRES           CPUTimeRAW                State                                            WorkDir
    ------------ ---------- ---------- --------- ---------- ----------- -------------------- -------------------- --------------------------------------------------
    52            E5-2640V4 tensorflow      lily         80                               80 COMPLETED            /home/lily/fds-test                                
    53            E5-2640V4 tensorflow      lily         80                                0 COMPLETED            /home/lily/fds-test                                
    54            E5-2640V4 tensorflow      lily         80                                0 COMPLETED            /home/lily/fds-test                                
    55            E5-2640V4 tensorflow      lily         80                                0 COMPLETED            /home/lily/fds-test                                
    56            E5-2640V4 tensorflow      lily         80                                0 COMPLETED            /home/lily/fds-test                                
    57            E5-2640V4 tensorflow      lily          4                                0 COMPLETED            /home/lily/fds-test                                
    58            E5-2640V4 tensorflow      lily        160                                0 CANCELLED by 1002    /home/lily/fds-test                                
    59            E5-2640V4 tensorflow      lily         80                                0 COMPLETED            /home/lily/fds-test                                
    60            E5-2640V4 tensorflow      lily         80                                0 COMPLETED            /home/lily/fds-test                                
    61            E5-2640V4 tensorflow      lily         80                                0 FAILED               /home/lily/fds-test                                
    62            E5-2640V4 tensorflow      lily         80                             2640 CANCELLED by 1002    /home/lily/fds-test                                
    63            E5-2640V4 tensorflow      lily         80                             5360 CANCELLED by 1002    /home/lily/fds-test                                
    64            E5-2640V4 tensorflow      lily         80                             7680 CANCELLED            /home/lily/fds-test                                
    65            E5-2640V4 tensorflow      lily         80                            18480 CANCELLED            /home/lily/fds-test                                
    66            E5-2640V4 tensorflow      lily         80                             3040 CANCELLED            /home/lily/fds-test                                
    67            E5-2640V4 tensorflow      lily         80                             7920 CANCELLED by 1002    /home/lily/fds-test                                
    68            E5-2640V4 tensorflow      lily         80                            10960 CANCELLED by 1002    /home/lily/fds-test                                
    69            E5-2640V4 tensorflow      lily         80                             5040 CANCELLED by 1002    /home/lily/fds-test                                
    70            E5-2640V4 tensorflow      lily         80                              800 COMPLETED            /home/lily/fds-test                                
    71            E5-2640V4 tensorflow      lily         80                              880 COMPLETED            /home/lily/fds-test                                
    72            E5-2640V4 tensorflow      lily         80                              800 COMPLETED            /home/lily/fds-test                                
    73            E5-2640V4 tensorflow      lily         80                            19760 CANCELLED by 1002    /home/lily/fds-test                                
    74            E5-2640V4 tensorflow      lily         80                                0 CANCELLED by 1002    /home/lily/fds-test2

同样，对第7列求和可以得到该段时间内的总机时：

    $ slhist -S 2019-01-01T00:00:00 |awk 'BEGIN{total=0}{total+=$7}END{print total}'



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

#### 1.5.1 error: Unable to allocate resources: Invalid account or account/partition combination specified

管理员创建用户时，未将该用户添加到任何Account中，使得该用户无法使用计算资源。请联系管理员，让管理员参考[2.2 Slurm系统管理](#22-slurm%E7%B3%BB%E7%BB%9F%E7%AE%A1%E7%90%86)进行设置。


[ssh远程连接集群](User/ssh远程连接集群.md)

[远程桌面连接集群](User/使用x2go连接Ubuntu远程桌面.md)

[Slurm用户教程](User/Slurm用户教程.md)



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
