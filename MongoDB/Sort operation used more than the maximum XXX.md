> 原文链接：<https://blog.csdn.net/u012031380/article/details/74316259>

<font color=red>Query failed with error code 96 and error message 'Executor error during find command: OperationFailed: Sort operation used more than the maximum 33554432 bytes of RAM. Add an index, or specify a smaller limit.' on server</font>

刚开始还以为是查询得到的内容比较大，所以指定字段即指定查询返回的字段，我用的是`Spring data mongo`的 query()，大致方法如下：

	 Query query = Query.query(new Criteria());
	 PageRequest pageRequest=new PageRequest(pageNumber - 1, pageSize);
	 query.addCriteria(Criteria.where("fillStatus").gte(10));
	 query.skip(pageRequest.getOffset()).limit(pageSize).with(new Sort(Sort.Direction.DESC, "createTime"));
	 long totalCount=this.mongoDao.count(query,HealthQues.class);
	 int totalPage=0;
	 if(totalCount%pageSize==0){
	     totalPage=Integer.parseInt(totalCount/pageSize+"");
	 }else{
	     totalPage=Integer.parseInt(totalCount/pageSize+"")+1;
	 }
	//指定返回字段
	query.fields().include("id");
	query.fields().include("memberId");

等等、、、这里指定一个就返回一个，指定俩个就返回两个。

	List<HealthQues> healthQuesList=this.mongoDao.find(query,HealthQues.class);//程序运行到这里报错！结果查询到仍然报错！！！

原因比较明确：`Sort operation used more than the maximum 33554432 bytes of RAM.`，33554432 bytes算下来正好是32Mb，而mongodb的sort操作是把数据拿到内存中再进行排序的，为了节约内存，默认给sort操作限制了最大内存为32Mb，当数据量越来越大直到超过32Mb的时候就自然抛出异常了！解决方案有两个思路，一个是既然内存不够用那就修改默认配置多分配点内存空间；一个是像错误提示里面说的那样创建索引。

首先说如何修改默认内存配置，在Mongodb命令行窗口中执行如下命令即可：

	db.adminCommand({setParameter:1, internalQueryExecMaxBlockingSortBytes:335544320}) //不推荐使用

我直接把内存扩大了10倍，变成了320Mb。从这里可以看出，除非你服务器的内存足够大，否则sort占用的内存会成为一个严重的资源消耗！

然后是创建索引，也比较简单：

	db.yourCollection.createIndex({<field>:<1 or -1>})  //只执行这句就能解决bug  注意不能直接复制粘贴，要根据集合编写语句。
	db.yourCollection.getIndexes()  //查看当前collection的索引

其中1表示升序排列，-1表示降序排列。索引创建之后即时生效，不需要重启数据库和服务器程序，也不需要对原来的数据库查询语句进行修改。创建索引的话也有不好的地方，会导致数据写入变慢，同时Mongodb数据本身占用的存储空间也会变多。不过从查询性能和服务器资源消耗这两方面来看，通过创建索引来解决这个问题还是最佳的方案！