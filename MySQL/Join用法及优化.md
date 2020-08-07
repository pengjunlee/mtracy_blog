> 转载自：<https://www.cnblogs.com/aeolian/p/10048821.html>

# Join的几种类型
<div align=center>

![mysql图](./imgs/61.jpg "mysql示意图")
<div align=left>

## 笛卡尔积(交叉连接) 
如果A表有n条记录，B表有m条记录，笛卡尔积产生的结果就会产生n*m条记录。
在MySQL中可以为CROSS JOIN或者省略CROSS即JOIN，或者直接用from多表用逗号分开。
如 

	SELECT * FROM table1 CROSS JOIN table2 
	SELECT * FROM table1 JOIN table2 
	SELECT * FROM table1 , table2

不用`on table1.key1 = table2.key2` 得出的结果是`table1的记录数`*`table2的记录数`，如果用on连接，得出的和inner join的结果一样(所以<font color=red>在有on的情况下，inner joijn、cross join、 join（推荐、会自动用小的表作为驱动表）结果一样</font>)。

## 内连接：INNER JOIN
内连接`INNER JOIN`是最常用的连接操作。从数学的角度讲就是求两个表的交集，从笛卡尔积的角度讲就是从笛卡尔积中挑出ON子句条件成立的记录。有INNER JOIN，WHERE（等值连接），STRAIGHT_JOIN,JOIN(省略INNER)四种写法。

## 左【外】连接：LEFT [out] JOIN
左连接`LEFT JOIN`的含义就是求两个表的交集外加左表剩下的数据。依旧从笛卡尔积的角度讲，就是先从笛卡尔积中挑出ON子句条件成立的记录，然后加上左表中剩余的记录（见最后三条）。

## 右【外】连接：RIGHT [out] JOIN
同理右连接`RIGHT JOIN`就是求两个表的交集外加右表剩下的数据。再次从笛卡尔积的角度描述，右连接就是从笛卡尔积中挑出ON子句条件成立的记录，然后加上右表中剩余的记录（见最后一条）。

## 全外连接：Full outer join
产生A和B的并集。对于没有匹配的记录，则以null做为值。(注意！！！Orcal有，Mysql是没有全外连接的！！！可以用做外连接和右外连接在union(union连接的是两个查询不是两张表)起来)

	select *,if(a.date is null,b.date,a.date) as date from a left join b on a.date = b.date  # -- 只有a的全部
	union # -- union上面的复制下来left改为right，连个表头要一致
	select *,if(a.date is null,b.date,a.date) as date from a right join b on a.date = b.date

# 查询存在于一张表不存在于另一张表的sql

	select ta.* from ta where ta.id not in(select tb.id from tb)  /*效率低*/
	select ta.id from ta left join tb on ta.id=tb.id where tb.id is NULL  /*优化*/

# 两表JOIN优化

- 当无order by条件时，根据实际情况，使用left/right/inner join即可，根据explain优化 ；
- 当有order by条件时，如select * from a inner join b where 1=1 and other condition order by a.col；使用explain解释语句；

1）如果第一行的驱动表为a，则效率会非常高，无需优化；  
2）否则，因为只能对驱动表字段直接排序的缘故，会出现using temporary，所以此时需要使用STRAIGHT_JOIN明确a为驱动表，来达到使用a.col上index的优化目的；或者使用left join且Where条件中不含b的过滤条件，此时的结果集为a的全集，而STRAIGHT_JOIN为inner join且使用a作为驱动表