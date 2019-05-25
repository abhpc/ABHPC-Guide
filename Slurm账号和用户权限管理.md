### 1. 账户（Account）和用户（User）

在进行管理之前，必须知道Slurm调度系统分为两个基本层面：Account（账户）和User（用户）。
基于这两个基本层面，Slurm形成了“关联”(Assoc)。

Account类似于group的概念，是由多个User（用户）组成，在大集群上可以用来统一计算用户组的
机时，通过对Account设置，可以对该用户组的全部用户进行条件约束。例如：在集群上需要限制某
个用户组的使用总核数，则可以将这些用户放置在一个Account下，设置其可用的CPU总数、GRES等
资源的限制。

User则落实到具体的某个用户，除了可以在Account上对全体用户进行限制外，还可以单独限制某个
用户的最大资源。

#### 显示全部关联关系：

    # sacctmgr list assoc
      Cluster    Account       User  Partition     Share GrpJobs       GrpTRES GrpSubmit     GrpWall   GrpTRESMins MaxJobs       MaxTRES MaxTRESPerNode MaxSubmit     MaxWall   MaxTRESMins             QOS   Def QOS GrpTRESRunMin
    ---------- ---------- ---------- ---------- --------- ------- ------------- --------- ----------- ------------- ------- ------------- -------------- --------- ----------- ------------- -------------------- --------- -------------
      abhpc-ai       root                               1                                                                                                                                                  normal                         
      abhpc-ai       root       root                    1                                                                                                                                                  normal                         
      abhpc-ai tensorflow                               1                                                                                                                                                  normal                         
      abhpc-ai tensorflow      abhpc                    1                                                                                                                                                  normal                         
      abhpc-ai tensorflow       lily                    1                                                                                                                                                  normal                         

这样显示未免过于凌乱，可以使用以下命令简洁地输出关联信息：

    # slassoc
       Cluster    Account       User                  QOS  Partition       GrpTRES
    ---------- ---------- ---------- -------------------- ---------- -------------
      abhpc-ai       root                          normal                          
      abhpc-ai       root       root               normal                          
      abhpc-ai tensorflow                          normal                          
      abhpc-ai tensorflow      abhpc               normal                          
      abhpc-ai tensorflow       lily               normal

### 2. 节点和队列


### 3. GRES与GPU调度
