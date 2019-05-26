### Fire Dynamics Simulator(FDS)的安装

本例子以安装FDS 6.7.5为例

#### 1. 软件简介

Fire Dynamics Simulator (FDS) is a large-eddy simulation (LES) code for low-speed flows, with an emphasis on smoke and heat transport from fires.

Smokeview (SMV) is a visualization program used to display the output of FDS and CFAST simulations.

项目开源地址：https://github.com/firemodels/fds

#### 2. 软件安装

##### 2.1 安装Intel Parallel Studio 2017

该软件依赖于Intel Parallel Studio 2017, 在安装之前需要首先安装好这个软件。

##### 2.2 下载并安装FDS 6.7.5

下载地址：https://github.com/firemodels/fds/releases

下载FDS6.7.1-SMV6.7.5_linux64.sh。

安装

    # bash FDS6.7.1-SMV6.7.5_linux64.sh

根据提示，一步一步设置，安装到/opt/FDS目录下，安装好后，确定能够让全部用户都可以使用：

    # chmod -Rf 755 /opt/FDS

然后在/opt/config/common.sh文件下添加以下两行：

    # FDS
    source /opt/FDS/FDS6/bin/FDS6VARS.sh

这样全部用户都可以使用了，仍然在登录的用户未必能够马上生效，需要登出一次，然后再登入即可生效。FDS的作业脚本[参见这里](常用slurm脚本/fds.slm)。
