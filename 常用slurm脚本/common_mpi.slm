#! /bin/bash

# 本脚本适用于一般MPI程序

# 这里指定作业名称
#SBATCH --job-name=test

# 提交到哪个队列（分区）
#SBATCH --partition=E5-2640V4

# 使用多少个节点
#SBATCH --nodes=4

# 每个节点使用多少核
#SBATCH --ntasks-per-node=20

# 错误和输出文件
#SBATCH --error=%j.err
#SBATCH --output=%j.out

# MPI程序命令，这里写mpirun后的ARGS即可
# 例如，单机上运行命令为mpirun -np 2 a.out -in xxx.in
# 这里MPI_CMD则写作“a.out -in xxx.in”
MPI_CMD="a.out -in xxx.in"

# 以下行如果不懂，可以不管，按默认的即可。如果你知道其含义的话，可以进行自定义修改。

# 以下生成MPI的nodelist
CURDIR=`pwd`
rm -rf $CURDIR/nodelist.$SLURM_JOB_ID
NODES=`scontrol show hostnames $SLURM_JOB_NODELIST`
for i in $NODES
do
	echo "$i:$SLURM_NTASKS_PER_NODE" >> $CURDIR/nodelist.$SLURM_JOB_ID
done
# 生成nodelist结束

# 通过MPI运行LAMMPS
mpirun -genv I_MPI_FABRICS=tcp -machinefile $CURDIR/nodelist.$SLURM_JOB_ID $MPI_CMD > $SLURM_JOB_NAME.sta

# 运行完后清理nodelist
rm -rf $CURDIR/nodelist.$SLURM_JOB_ID
