### 1.Ubuntu 16.04LTS

#### 1.1 安装MATE桌面环境

    # apt install -y ubuntu-mate-desktop

此过程也许持续10分钟左右，请耐心等待。

#### 1.2 安装x2go服务器

    # apt-get -y install software-properties-common
    # add-apt-repository ppa:x2go/stable
    # apt-get update
    # apt-get install -y x2goserver x2goserver-xsession

安装完成，使用方法参考[使用x2go连接MATE远程桌面](../User/使用x2go连接MATE远程桌面.md)。
