### 1.提交作业

    $ sbatch [作业脚本].slm

### 2.查看作业

    $ squeue

通过此命令可以获得作业的信息如ID和运行状态等。

### 3.中断作业

    $ scancel [作业ID]

中断本用户的全部作业

    $ scancel -u $USER

### 4.查看队列情况
