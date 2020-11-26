> 原文地址： <https://blog.csdn.net/tdmyl/article/details/31811317>

# 下载

hadoop使用`protocol buffer`进行通信，需要下载和安装`protobuf-2.5.0.tar.gz`。由于现在`protobuf-2.5.0.tar.gz`已经无法在[官网](官网 "https://code.google.com/p/protobuf/downloads/list")中下载了，本人将`protobuf-2.5.0.tar.gz`上传到百度云盘供大家下载，地址：<http://pan.baidu.com/s/1pJlZubT>。

# 安装

使用`tar -zxf protobuf-2.5.0.tar.gz`命令解压后得到是`protobuf-2.5.0`的源码，

    cd protobuf-2.5.0 #进入目录

假如你希望编译成功后输出的目录为`/home/work/protobuf/`则输入如下两条命令：

    ./configure --prefix=/home/work/protobuf/  
    make && make install

编译成功后将`export PATH=/home/work/protobuf/bin:$PATH`加入到环境变量中

最后输入`protoc --version`命令，如显示`libprotoc 2.5.0`则安装成功