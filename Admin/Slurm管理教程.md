在进行管理之前，必须知道Slurm调度系统分为两个基本层面：Account（账户）和User（用户）。基于这两个基本层面，Slurm形成了“关联”(Assoc)。

Account类似于group的概念，是由多个User（用户）组成，在大集群上可以用来统一计算用户组的机时，通过对Account设置，可以对该用户组的全部用户进行条件约束。例如：在集群上需要限制某个用户组的使用总核数，则可以将这些用户放置在一个Account下，设置其可用的CPU总数、GRES等资源的限制。

User则落实到具体的某个用户，除了可以在Account上对全体用户进行限制外，还可以单独限制某个用户的最大资源。

### 1. 查看关联关系

##### 1.1 查看全部关联关系

    # sacctmgr list assoc
       Cluster    Account       User  Partition     Share GrpJobs       GrpTRES GrpSubmit     GrpWall   GrpTRESMins MaxJobs       MaxTRES MaxTRESPerNode MaxSubmit     MaxWall   MaxTRESMins             QOS   Def QOS GrpTRESRunMin
    ---------- ---------- ---------- ---------- --------- ------- ------------- --------- ----------- ------------- ------- ------------- -------------- --------- ----------- ------------- -------------------- --------- -------------
      abhpc-ai       root                               1                                                                                                                                                  normal                         
      abhpc-ai       root       root                    1                                                                                                                                                  normal                         
      abhpc-ai tensorflow                               1                                                                                                                                                  normal                         
      abhpc-ai tensorflow      abhpc                    1                                                                                                                                                  normal                         
      abhpc-ai tensorflow       lily                    1                                                                                                                                                  normal                         

##### 1.2 slassoc命令

全部显示关联未免过于凌乱，可以使用以下命令简洁地输出关联信息：

    # slassoc
       Cluster    Account       User                  QOS  Partition       GrpTRES
    ---------- ---------- ---------- -------------------- ---------- -------------
      abhpc-ai       root                          normal                          
      abhpc-ai       root       root               normal                          
      abhpc-ai tensorflow                          normal                          
      abhpc-ai tensorflow      abhpc               normal                          
      abhpc-ai tensorflow       lily               normal

### 2. Account和User的管理

通过以下命令可以查看到Account管理的参数：

    # sacctmgr --help
    ......
    add account        - Clusters=, DefaultQOS=, Description=, Fairshare=,
                         GrpTRESMins=, GrpTRES=, GrpJobs=, GrpMemory=,   
                         GrpNodes=, GrpSubmitJob=, GrpWall=, MaxTRESMins=,
                         MaxTRES=, MaxJobs=, MaxNodes=, MaxSubmitJobs=,
                         MaxWall=, Names=, Organization=, Parent=,      
                         and QosLevel=                                  
    modify account     - (set options) DefaultQOS=, Description=,       
                         Fairshare=, GrpTRESMins=, GrpTRESRunMins=,       
                         GrpTRES=, GrpJobs=, GrpMemory=, GrpNodes=,     
                         GrpSubmitJob=, GrpWall=, MaxTRESMins=, MaxTRES=,
                         MaxJobs=, MaxNodes=, MaxSubmitJobs=, MaxWall=,
                         Names=, Organization=, Parent=, and QosLevel=  
                         RawUsage= (with admin privileges only)         
                         (where options) Clusters=, DefaultQOS=,        
                         Descriptions=, Names=, Organizations=,         
                         Parent=, and QosLevel=
    ......

同样通过以下命令可以查看到add user部分的参数：

    # sacctmgr --help
    ......
    add user           - Accounts=, AdminLevel=, Clusters=,             
                         DefaultAccount=, DefaultQOS=, DefaultWCKey=,   
                         Fairshare=, MaxTRESMins=, MaxTRES=,            
                         MaxJobs=, MaxNodes=, MaxSubmitJobs=, MaxWall=,
                         Names=, Partitions=, and QosLevel=             
    modify user        - (set options) AdminLevel=, DefaultAccount=,    
                         DefaultQOS=, DefaultWCKey=, Fairshare=,        
                         MaxTRESMins=, MaxTRES=, MaxJobs=, MaxNodes=,   
                         MaxSubmitJobs=, MaxWall=, NewName=,            
                         and QosLevel=,                                 
                         RawUsage= (with admin privileges only)         
                         (where options) Accounts=, AdminLevel=,        
                         Clusters=, DefaultAccount=, Names=,            
                         Partitions=, and QosLevel=
    ......

##### 2.1 新建一个account的命令如下：

    # sacctmgr add account [账号名称]

##### 2.2 添加用户到指定的Account（例如tensorflow）：

    # sacctmgr add user account=tensorflow

##### 2.3 修改用户属性

    # sacctmgr modify user [用户名] set [属性]=[设定值]

用户的属性值可以查阅[帮助文档](https://slurm.schedmd.com/sacctmgr.html)。

### 3. Account和User的权限管理

Account和User的权限使用GrpTRES来进行限制：

GrpTRES=<TRES=max TRES,...>  
Maximum number of TRES running jobs are able to be allocated in aggregate for this association and all associations which are children of this association. To clear a previously set value use the modify command with a new value of -1 for each TRES id.  
NOTE: This limit only applies fully when using the Select Consumable Resource plugin.

例如，限制lily用户的CPU核数为40，节点数为2，GPU最大使用为4：

    # sacctmgr modify user lily set Grptres="cpu=40,node=2,gres/gpu=4"
    # slassoc
       Cluster    Account       User                  QOS  Partition                        GrpTRES
    ---------- ---------- ---------- -------------------- ---------- ------------------------------
      abhpc-ai       root                          normal
      abhpc-ai       root       root               normal
      abhpc-ai tensorflow                          normal
      abhpc-ai tensorflow      abhpc               normal
      abhpc-ai tensorflow       lily               normal                  cpu=40,gres/gpu=4,node=2

重置该用户全部限制则为：

    # sacctmgr modify user lily set Grptres="cpu=-1,node=-1,gres/gpu=-1"
       Cluster    Account       User                  QOS  Partition                        GrpTRES
    ---------- ---------- ---------- -------------------- ---------- ------------------------------
      abhpc-ai       root                          normal
      abhpc-ai       root       root               normal
      abhpc-ai tensorflow                          normal
      abhpc-ai tensorflow      abhpc               normal
      abhpc-ai tensorflow       lily               normal                  

同样，可以对整个Account进行限制，其含义是Account包含的全部用户累计总和不得超过最大限制。例如：

    # sacctmgr modify account tensorflow set Grptres="cpu=40,node=2,gres/gpu=4"
    # slassoc
       Cluster    Account       User                  QOS  Partition                        GrpTRES
    ---------- ---------- ---------- -------------------- ---------- ------------------------------
      abhpc-ai       root                          normal                                           
      abhpc-ai       root       root               normal                                           
      abhpc-ai tensorflow                          normal                  cpu=40,gres/gpu=4,node=2
      abhpc-ai tensorflow      abhpc               normal                                           
      abhpc-ai tensorflow       lily               normal

重置该Account的全部限制为：

    # sacctmgr modify account tensorflow set Grptres="cpu=-1,node=-1,gres/gpu=-1"
       Cluster    Account       User                  QOS  Partition                        GrpTRES
    ---------- ---------- ---------- -------------------- ---------- ------------------------------
      abhpc-ai       root                          normal
      abhpc-ai       root       root               normal
      abhpc-ai tensorflow                          normal
      abhpc-ai tensorflow      abhpc               normal
      abhpc-ai tensorflow       lily               normal

### 4. 管理员计费系统

如果集群涉及到计费问题，则需要统计用户(User)或账户(Account)在某段时间内的使用机时，在[用户教程](../User/Slurm用户教程.md)中已经提供了用户机时的统计方法，但同一Account下的其他用户的作业是互相不可见的，因此，作为管理员可以对单个用户计费，也可以对整个Account进行机时计费，以下举例说明。

##### 4.1 对用户lily自2019年1月1日0时起使用的机时进行统计：

    # sacct -u lily -S 2019-01-01T00:00:00 -o "jobid,partition,account,user,alloccpus,cputimeraw,state,workdir%60" -X |awk 'BEGIN{total=0}{total+=$6}END{print total}'

##### 4.2 对名为tensorflow的account自2019年1月1日0时起使用的机时进行统计：

    # sacct -A tensorflow -S 2019-01-01T00:00:00 -o "jobid,partition,account,user,alloccpus,cputimeraw,state,workdir%60" -X |awk 'BEGIN{total=0}{total+=$6}END{print total}'

注意这里的机时单位都是秒，换算成核时需要除以3600。
