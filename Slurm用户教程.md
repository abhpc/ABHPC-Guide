### 1. 提交作业

    $ sbatch [作业脚本].slm

常用的slurm脚本可以在[这里下载](常用slurm脚本)。

### 2. 查看作业

    $ squeue

通过此命令可以获得作业的信息如ID和运行状态等。

### 3. 中断作业

    $ scancel [作业ID]

中断本用户的全部作业

    $ scancel -u $USER

### 4. 查看节点情况

    $ slhosts
    HOSTNAME STATE      CPUS    CPUS(A/I/O/T) FREE_MEM   REASON GRES
    C01      idle       20          0/20/0/20    52609     none gpu:rtx2080:2
    C02      idle       20          0/20/0/20    53734     none gpu:rtx2080:2
    C03      idle       20          0/20/0/20    53692     none gpu:rtx2080:2
    C04      idle       20          0/20/0/20    53740     none gpu:rtx2080:2

CPUS(A/I/O/T)分别代表CPU的Allocated/Idle/Other/Total个数。这样可以判断哪些节点是可用
的，最大能提交多少核、多少节点。

### 5. 查看队列(Partition)状态

    $ spartitions
    PARTITION  AVAIL    PRIO_TIER    MAX_CPUS_PER_NODE      NODES(A/I/O/T)
    E5-2640V4* up       100          20                            0/4/0/4

NODES(A/I/O/T)分别代表节点的Allocated/Idle/Other/Total个数。这样可以判断哪些队列是可
用的，最大能提交多少核、多少节点。

### 6. 历史作业信息与统计

Slurm的时间格式为：[YYYY]-[MM]-[DD]T[hh]:[mm]:[ss]，如2019年1月1日0时0分0秒的格式写作：

    2019-01-01T00:00:00

查看历史作业信息使用sacct命令进行查看，时间范围通过"-S"指定起始时间，"-E"指定结束时间。默认起始时间是当日0时，默认结束时间是当前时间。

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
