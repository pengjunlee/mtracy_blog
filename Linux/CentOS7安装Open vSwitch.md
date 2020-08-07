> 转载自：<https://www.jianshu.com/p/658332deac99>

最近在研究Docker网络方案，需要安装OVS，记录下安装配置过程

# 1.关闭SELINUX

	#永久关闭SELINUX
	编辑/etc/selinux/config文件，并设置SELINUX=disabled，然后重启生效
	    
	#临时关闭SELINUX
	setenforce 0
	    
	#临时打开SELINUX
	setenforce 1
	    
	#验证SELINUX状态（Permissive-关闭，Enforcing-打开）
	getenforce

# 2.安装依赖包

	#yum -y install make gcc openssl-devel autoconf automake rpm-build redhat-rpm-config
	
	#yum -y install python-devel openssl-devel kernel-devel kernel-debug-devel libtool wget

# 3.下载openvswitch软件包
[官网下载](https://link.jianshu.com/?t=http://www.openvswitch.org/download/ "官网下载")   
[官方安装指导手册](https://link.jianshu.com/?t=http://docs.openvswitch.org/en/latest/intro/install/ "官方安装指导手册")

	#wget http://openvswitch.org/releases/openvswitch-2.5.2.tar.gz

# 4.预处理

	#mkdir -p ~/rpmbuild/SOURCES
	#cp openvswitch-2.5.2.tar.gz ~/rpmbuild/SOURCES/ 
	#cd ~/rpmbuild/SOURCES
	#tar xvfz openvswitch-2.5.2.tar.gz
	#sed 's/openvswitch-kmod, //g' openvswitch-2.5.2/rhel/openvswitch.spec > openvswitch-2.5.2/rhel/openvswitch_no_kmod.spec 

# 5.构建RPM包

	#rpmbuild -bb --nocheck openvswitch-2.5.2/rhel/openvswitch_no_kmod.spec

# 6.安装

	#yum localinstall ~/rpmbuild/RPMS/x86_64/openvswitch-2.5.2-1.x86_64.rpm

# 7.启动服务

	#service openvswitch restart
	Restarting openvswitch (via systemctl):                    [  OK  ]

# 8.检查OVS服务状态

	#service openvswitch status
	ovsdb-server is running with pid 4567
	ovs-vswitchd is running with pid 4581

# 9.查看日志

	# tail /var/log/messages
	Jun 20 17:40:27 10-1-239-44 openvswitch: Creating empty database /etc/openvswitch/conf.db [  OK  ]
	Jun 20 17:40:27 10-1-239-44 openvswitch: Starting ovsdb-server [  OK  ]
	Jun 20 17:40:27 10-1-239-44 ovs-vsctl: ovs|00001|vsctl|INFO|Called as ovs-vsctl --no-wait -- init -- set Open_vSwitch . db-version=7.12.1
	Jun 20 17:40:27 10-1-239-44 ovs-vsctl: ovs|00001|vsctl|INFO|Called as ovs-vsctl --no-wait set Open_vSwitch . ovs-version=2.5.2 "external-ids:system-id=\"bb8275bd-f310-4b3e-a8e8-5fad4921881a\"" "system-type=\"unknown\"" "system-version=\"unknown\""
	Jun 20 17:40:27 10-1-239-44 openvswitch: Configuring Open vSwitch system IDs [  OK  ]
	Jun 20 17:40:27 10-1-239-44 openvswitch: Inserting openvswitch module [  OK  ]
	Jun 20 17:40:27 10-1-239-44 kernel: openvswitch: Open vSwitch switching datapath
	Jun 20 17:40:27 10-1-239-44 openvswitch: Starting ovs-vswitchd [  OK  ]
	Jun 20 17:40:27 10-1-239-44 openvswitch: Enabling remote OVSDB managers [  OK  ]
	Jun 20 17:40:27 10-1-239-44 systemd: Started LSB: Open vSwitch switch.
