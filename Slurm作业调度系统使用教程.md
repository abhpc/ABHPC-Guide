### 1.提交作业

    $ sbatch [作业脚本].slm

常用的slurm脚本可以在[这里下载](常用slurm脚本)。

### 2.查看作业

    $ squeue

通过此命令可以获得作业的信息如ID和运行状态等。

### 3.中断作业

    $ scancel [作业ID]

中断本用户的全部作业

    $ scancel -u $USER

### 4.查看节点情况

    # slhosts
    HOSTNAME STATE      CPUS    CPUS(A/I/O/T) FREE_MEM   REASON GRES
    C01      idle       20          0/20/0/20    52609     none gpu:rtx2080:2
    C02      idle       20          0/20/0/20    53734     none gpu:rtx2080:2
    C03      idle       20          0/20/0/20    53692     none gpu:rtx2080:2
    C04      idle       20          0/20/0/20    53740     none gpu:rtx2080:2

CPUS(A/I/O/T)分别代表CPU的Allocated/Idle/Other/Total个数。这样可以判断哪些节点是可用
的，最大能提交多少核、多少节点。

### 5.查看队列(Partition)状态

    # spartitions
    PARTITION  AVAIL    PRIO_TIER    MAX_CPUS_PER_NODE      NODES(A/I/O/T)
    E5-2640V4* up       100          20                            0/4/0/4

NODES(A/I/O/T)分别代表节点的Allocated/Idle/Other/Total个数。这样可以判断哪些队列是可
用的，最大能提交多少核、多少节点。
