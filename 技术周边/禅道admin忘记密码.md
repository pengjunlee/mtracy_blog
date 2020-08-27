> 原文地址： <https://www.jianshu.com/p/c6cc3cbe89a6>


	/opt/zbox/run/mysql/mysql -uroot -p
	# 禅道数据库root默认密码123456
	MariaDB [(none)]> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| mysql              |
	| performance_schema |
	| zentao             |
	| zentaobiz          |
	| zentaopro          |
	+--------------------+
	6 rows in set (0.00 sec)
	 
	MariaDB [(none)]> use zentao;
	Reading table information for completion of table and column names
	You can turn off this feature to get a quicker startup with -A
	 
	# 忘记admin密码后，修改zt_user表
	 
	MariaDB [zentao]> select id,account,password from zt_user;
	+----+---------+----------------------------------+
	| id | account | password                         |
	+----+---------+----------------------------------+
	|  1 | admin   | 070b17106ddfd497e1018e6a9990fb5f |
	+----+---------+----------------------------------+
	1 row in set (0.00 sec)
	 
	MariaDB [zentao]> update zt_user set password='e10adc3949ba59abbe56e057f20f883e' where id=1;
	Query OK, 1 row affected (0.00 sec)
	Rows matched: 1  Changed: 1  Warnings: 0
	 
	MariaDB [zentao]>
	 
	# e10adc3949ba59abbe56e057f20f883e 即：123456
 