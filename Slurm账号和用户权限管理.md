在进行管理之前，必须知道Slurm调度系统分为两个基本层面：Account（账户）和User（用户）。基于这两个基本层面，Slurm形成了“关联”(Assoc)。

Account类似于group的概念，是由多个User（用户）组成，在大集群上可以用来统一计算用户组的机时，通过对Account设置，可以对该用户组的全部用户进行条件约束。例如：在集群上需要限制某个用户组的使用总核数，则可以将这些用户放置在一个Account下，设置其可用的CPU总数、GRES等资源的限制。

User则落实到具体的某个用户，除了可以在Account上对全体用户进行限制外，还可以单独限制某个用户的最大资源。

#### 查看全部关联关系

    # sacctmgr list assoc
       Cluster    Account       User  Partition     Share GrpJobs       GrpTRES GrpSubmit     GrpWall   GrpTRESMins MaxJobs       MaxTRES MaxTRESPerNode MaxSubmit     MaxWall   MaxTRESMins             QOS   Def QOS GrpTRESRunMin
    ---------- ---------- ---------- ---------- --------- ------- ------------- --------- ----------- ------------- ------- ------------- -------------- --------- ----------- ------------- -------------------- --------- -------------
      abhpc-ai       root                               1                                                                                                                                                  normal                         
      abhpc-ai       root       root                    1                                                                                                                                                  normal                         
      abhpc-ai tensorflow                               1                                                                                                                                                  normal                         
      abhpc-ai tensorflow      abhpc                    1                                                                                                                                                  normal                         
      abhpc-ai tensorflow       lily                    1                                                                                                                                                  normal                         

#### slassoc命令

全部显示关联未免过于凌乱，可以使用以下命令简洁地输出关联信息：

    # slassoc
       Cluster    Account       User                  QOS  Partition       GrpTRES
    ---------- ---------- ---------- -------------------- ---------- -------------
      abhpc-ai       root                          normal                          
      abhpc-ai       root       root               normal                          
      abhpc-ai tensorflow                          normal                          
      abhpc-ai tensorflow      abhpc               normal                          
      abhpc-ai tensorflow       lily               normal

#### 新建Account

通过以下命令可以查看到add account部分的参数：

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

新建一个account的命令如下：

    # sacctmgr add account [账号名称]

#### 添加用户到指定的Account

通过以下命令可以查看到add user部分的参数：

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

##### 添加用户到指定的Account（例如tensorflow）：

    # sacctmgr add user account=tensorflow

##### 修改用户属性

    # sacctmgr modify user [用户名] set [属性]=[设定值]

用户的属性值可以查阅[帮助文档](https://slurm.schedmd.com/sacctmgr.html){:target="_blank"}。
