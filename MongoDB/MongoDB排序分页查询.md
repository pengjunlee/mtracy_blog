> 原文链接：<https://blog.csdn.net/qq_39198749/article/details/106077957>

# mongoDB中的使用
**示例文档**

	// 1
	{
	    "_id": "123",
	    "age": 25,
	    "likes": []
	}
	 
	// 2
	{
	    "_id": "456",
	    "age": 23,
	    "likes": []
	}

## $in
相当于sql中的in

	db.collcetion.find({"_id":{"$in":["123","456"]}})

## $gt
 

- (>) 大于 - $gt
- (<) 小于 - $lt
- (>=) 大于等于 - $gte
- (<= ) 小于等于 - $lte

	db.collcetion.find({"age":{"$gt":20}})
 
## $addToSet
添加值到一个数组中，如果该数组中已经存在该值，则不会添加；如果不存在该值，则添加

	db.collcetion.update({"_id":"123"}, {"$addToSet":{"likes":"PingPang"}})

如果想要批量添加list，需要配合`$each`使用

	db.collcetion.update({"_id":"123"}, {"$addToSet":{"likes":{"$each":["PingPang","FootBall","BasketBall"]}}})

## $elemMatch
**测试文档**

	// 1
	{
	    "_id": "123",
	    "user": [
	        {
	            "name": "qushuai1",
	            "address": "BeiJing",
	            "age": "22"
	        },
	        {
	            "name": "qushuai2",
	            "address": "TianJing",
	            "age": "23"
	        },
	        {
	            "name": "qushuai3",
	            "address": "ShangHai",
	            "age": "23"
	        },
	        {
	            "name": "qushuai4",
	            "address": "ShenZhen",
	            "age": "23"
	        }
	    ]
	}

mongoDB语句

	db.my_test.find({"_id":"123"},{"user":{$elemMatch:{"age":"22"}}})

返回结果

	{
	    "_id": "123",
	    "user": [
	        {
	            "name": "qushuai1",
	            "address": "BeiJing",
	            "age": "22"
	        }
	    ]
	}

<font color=red>注意</font>：使用$elemMatch时，只会返回仅仅匹配到的第一个合适的元素，所以对于数组中只有一个返回元素时，我们可以使用该操作符；对于返回多个元素时，则不可以使用。

下面介绍如何返回多个元素

	db.my_test.aggregate([{"$unwind":"$user"},
	{"$match":{"user.age":"23"}},
	{"$project":{"user":1}}])

返回

	// 1
	{
	    "_id": "123",
	    "user": {
	        "name": "qushuai2",
	        "address": "TianJing",
	        "age": "23"
	    }
	}
	 
	// 2
	{
	    "_id": "123",
	    "user": {
	        "name": "qushuai3",
	        "address": "ShangHai",
	        "age": "23"
	    }
	}
	 
	// 3
	{
	    "_id": "123",
	    "user": {
	        "name": "qushuai4",
	        "address": "ShenZhen",
	        "age": "23"
	    }
	}

`$unwind`：将数组中的每一个元素转为每一条文档

	db.my_test.aggregate([{"$unwind":"$user"}])

返回

	// 1
	{
	    "_id": "123",
	    "user": {
	        "name": "qushuai1",
	        "address": "BeiJing",
	        "age": "22"
	    }
	}
	 
	// 2
	{
	    "_id": "123",
	    "user": {
	        "name": "qushuai2",
	        "address": "TianJing",
	        "age": "23"
	    }
	}
	 
	// 3
	{
	    "_id": "123",
	    "user": {
	        "name": "qushuai3",
	        "address": "ShangHai",
	        "age": "23"
	    }
	}
	 
	// 4
	{
	    "_id": "123",
	    "user": {
	        "name": "qushuai4",
	        "address": "ShenZhen",
	        "age": "23"
	    }
	}

`$match`：简单的过滤文档，条件查询

	db.my_test.aggregate([{"$unwind":"$user"},{"$match":{"user.name":"qushuai2","_id":"123"}}])

返回

	// 1
	{
	    "_id": "123",
	    "user": {
	        "name": "qushuai2",
	        "address": "TianJing",
	        "age": "23"
	    }
	}

`$project`：修改输入文档的结构，也可以控制返回文档中哪些字段显示(1)、哪些字段不显示(0)，默认显示“_id“字段

	db.my_test.aggregate([{"$unwind":"$user"},{"$match":{"user.age":"23","_id":"123"}},{"$project":{"user":0}}])

返回

	// 1
	{
	    "_id": "123"
	}
	// 2
	{
	    "_id": "123"
	}
	// 3
	{
	    "_id": "123"
	}

## sort()排序
在`MongoDB`中使用`sort()`方法对数据进行排序，sort()方法可以通过参数指定排序的字段，并使用`1`和`-1`来指定排序的方式，其中`1`为升序排列，而`-1`是用于降序排列。

	db.scheduleInfo.find({},{}).sort({"property":1})

## 分页
**方法一**： 通过skip()、limit()配合使用(当数据量较大时，使用skip()效率会比较低)
不要轻易的使用skip()，因为skip是一条一条的数过来的，数据量比较大时，效率会比较慢

	db.scheduleInfo.find({},{}).sort({"property":1}).skip(2*5).limit(5)

**方法二**： 获取前一页的最后一条记录，查询之后的指定条记录

# MongoTemplate中的应用
首先了解下Criteria，基本文档的查询操作符
<table><thead><tr><th align="left">Criteria</th><th align="left">Mongodb</th><th align="left">说明</th></tr></thead><tbody><tr><td align="left">Criteria and (String key)</td><td align="left">$and</td><td align="left">并且</td></tr><tr><td align="left">Criteria orOperator (Criteria…&#8203; criteria)</td><td align="left">$or</td><td align="left">或者</td></tr><tr><td align="left">Criteria gt (Object o)</td><td align="left">$gt</td><td align="left">大于</td></tr><tr><td align="left">Criteria gte (Object o)</td><td align="left">$gte</td><td align="left">大于等于</td></tr><tr><td align="left">Criteria lt (Object o)</td><td align="left">$lt</td><td align="left">小于</td></tr><tr><td align="left">Criteria lte (Object o)</td><td align="left">$lte</td><td align="left">小等于</td></tr><tr><td align="left">Criteria in (Object…&#8203; o)</td><td align="left">$in</td><td align="left">包含</td></tr><tr><td align="left">Criteria nin (Object…&#8203; o)</td><td align="left">$nin</td><td align="left">不包含</td></tr><tr><td align="left">。。。。。。</td><td align="left">。。。。。。</td><td align="left">。。。。。。</td></tr></tbody></table>

## $in
	Query query = new Query(Criteria.where("key").in(List));
## $gt
	Query query = new Query(Criteria.where("id").gt(123));
## $addToSet
	Query query = new Query(Criteria.where("_id").is(123))
	 Document updateDoc = new Document();
	 updateDoc.append("$addToSet", new Document().append("listKey", gameId));
	 BasicUpdate update = new BasicUpdate(updateDoc);
	 // 实现数据存在就更新，不存在就插入数据
	 mongoTemplate.upsert(query, update, docName);
## $elemMatch
	// 类似结构{"_id":"12345","listKey": [{"property1": "XXX1","property2": "XXX2"},{"property1": "YYY1","property2": "YYY2"}]}
	Criteria findModelCriteria = Criteria.where("_id").is("0");
	// listKey表示数组的key,property表示listKey下的一个元素中的属性。
	Criteria findFuelCriteria = Criteria.where("listKey").elemMatch(Criteria.where("property1").is("XXXX"));
	BasicQuery basicQuery = new BasicQuery(findModelCriteria.getCriteriaObject(), findFuelCriteria.getCriteriaObject());
	Document document= mongoTemplate.findOne(basicQuery, Document.class, docName);
## 排序、分页
此处转载博文： <https://blog.csdn.net/liao0801_123/article/details/95311837>

	package com.star.ac.mongodb.impl;
	 
	import java.util.ArrayList;
	import java.util.Date;
	import java.util.HashMap;
	import java.util.Iterator;
	import java.util.List;
	import java.util.Map;
	 
	import org.apache.commons.collections.CollectionUtils;
	import org.apache.commons.lang3.StringUtils;
	import org.apache.log4j.Logger;
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.data.domain.Sort;
	import org.springframework.data.mongodb.core.MongoTemplate;
	import org.springframework.data.mongodb.core.aggregation.Aggregation;
	import org.springframework.data.mongodb.core.aggregation.AggregationResults;
	import org.springframework.data.mongodb.core.aggregation.DateOperators;
	import org.springframework.data.mongodb.core.aggregation.GroupOperation;
	import org.springframework.data.mongodb.core.aggregation.MatchOperation;
	import org.springframework.data.mongodb.core.aggregation.ProjectionOperation;
	import org.springframework.data.mongodb.core.aggregation.SortOperation;
	import org.springframework.data.mongodb.core.aggregation.TypedAggregation;
	import org.springframework.data.mongodb.core.mapreduce.GroupBy;
	import org.springframework.data.mongodb.core.mapreduce.GroupByResults;
	import org.springframework.data.mongodb.core.query.BasicQuery;
	import org.springframework.data.mongodb.core.query.Criteria;
	import org.springframework.data.mongodb.core.query.Query;
	import org.springframework.stereotype.Repository;
	 
	import com.alibaba.fastjson.JSON;
	import com.star.ac.dto.AlarmStatisDto;
	import com.star.ac.dto.CommunitySecurityStatis;
	import com.star.ac.dto.CommunitySecurityStatisData;
	import com.star.ac.dto.DeviceAlarmStatisDto;
	import com.star.ac.dto.SingleAlarm;
	import com.star.ac.model.AlarmInfo;
	import com.star.ac.model.AlarmStatisPerDay;
	import com.star.ac.model.BreakdownInfo;
	import com.star.ac.mongodb.AlarmMongodbDao;
	import com.star.ac.param.AlarmParam;
	import com.star.ac.param.AlarmStatisParam;
	import com.star.ac.param.AlarmUpToDateParam;
	import com.star.ac.param.CommunitySecurityParam;
	import com.star.ac.param.DeviceAlarmStatisParam;
	import com.star.common.utils.DateUtil;
	import com.star.common.utils.StringUtil;
	import com.mongodb.BasicDBObject;
	import com.mongodb.DBObject;
	 
	@Repository
	public class AlarmMongodbDaoImpl implements AlarmMongodbDao {
	 
	    private static final Logger logger = Logger.getLogger(AlarmMongodbDaoImpl.class);
	 
	    @Autowired
	    private MongoTemplate mongoTemplate;
	 
	 
	 
	    /**
	     * @Description: 分页查询
	     * @Author: 
	     * @Date: 2019/5/8 9:59
	     */    
	    @Override
	    public Map<String, Object> getAlarmLogByDeviceCode(AlarmParam param) {
	        logger.info("AlarmMongodbDaoImpl.getAlarmLogByDeviceCode start");
	 
	        Map<String, Object> resultMap = new HashMap<>();
	        int currentPage = param.getCurPage();
	        int pageSize = param.getPageSize();
	 
	        // 设置字段
	        // 创建查询及条件
	        Query query = Query.query(Criteria.where("spCode").is(param.getSpCode()));
	        if (!StringUtil.isNil(param.getDeviceCode())) {
	            query.addCriteria(Criteria.where("deviceCode").is(param.getDeviceCode()));
	        }
	 
	        // 设置起始数
	        query.skip((currentPage - 1) * pageSize)
	                // 设置查询条数
	                .limit(pageSize);
	 
	        // 查询记录总数
	        int totalCount = (int) mongoTemplate.count(query, AlarmInfo.class);
	        // 数据总页数
	        int totalPage = totalCount % pageSize == 0 ? totalCount / pageSize : totalCount / pageSize + 1;
	 
	        // 设置记录总数和总页数
	        resultMap.put("totalCount", totalCount);
	        resultMap.put("totalPage", totalPage);
	 
	        // 按创建时间倒序
	        query.with(new Sort(Sort.Direction.DESC, "createDate"));
	        // 查询当前页数据集合
	        List<AlarmInfo> records = mongoTemplate.find(query, AlarmInfo.class);
	        resultMap.put("data", records);
	        return resultMap;
	    }
	 
	    /**
	     * 分页查询
	     */
	    @Override
	    public Map<String, Object> getBreakdownLogByDeviceCode(AlarmParam param) {
	        logger.info("AlarmMongodbDaoImpl.getBreakdownLogByDeviceCode start");
	 
	        Map<String, Object> resultMap = new HashMap<>();
	        int currentPage = param.getCurPage();
	        int pageSize = param.getPageSize();
	 
	        // 创建查询及条件
	        Query query = Query.query(Criteria.where("spCode").is(param.getSpCode()));
	 
	        if (!StringUtil.isNil(param.getDeviceCode())) {
	            query.addCriteria(Criteria.where("deviceCode").is(param.getDeviceCode()));
	        }
	 
	        // 设置起始数
	        query.skip((currentPage - 1) * pageSize)
	                // 设置查询条数
	                .limit(pageSize);
	 
	        // 查询记录总数
	        int totalCount = (int) mongoTemplate.count(query, BreakdownInfo.class);
	        // 数据总页数
	        int totalPage = totalCount % pageSize == 0 ? totalCount / pageSize : totalCount / pageSize + 1;
	 
	        // 设置记录总数和总页数
	        resultMap.put("totalCount", totalCount);
	        resultMap.put("totalPage", totalPage);
	 
	        // 按创建时间倒序
	        query.with(new Sort(Sort.Direction.DESC, "createDate"));
	        // 查询当前页数据集合
	        List<BreakdownInfo> records = mongoTemplate.find(query, BreakdownInfo.class);
	        resultMap.put("data", records);
	        return resultMap;
	    }
	 
	    /*
	     * 指定字段 且返回最新的数据
	     */
	    @Override
	    public List<AlarmInfo> getAlarmStatisUpToDate(AlarmUpToDateParam param) {
	        logger.info("AlarmMongodbDaoImpl.getAlarmStatisUpToDate start");
	        BasicDBObject dbObject = new BasicDBObject();
	        // dbObject.put("content", "content1");
	        // dbObject.put("level", 888); //相当于条件 level = 888
	        // dbObject.append(key, val);
	 
	        // 指定返回的字段
	        BasicDBObject fieldsObject = new BasicDBObject();
	        fieldsObject.put("content", true);
	        fieldsObject.put("type", true);
	        fieldsObject.put("level", true);
	        fieldsObject.put("createDate", true);
	 
	        Query query = new BasicQuery(dbObject.toJson(), fieldsObject.toJson());
	 
	        String today = DateUtil.date2string(new Date()).substring(0, 10);
	        // 查询时间 mongoDB存储的是UTC时间 , 但读取和条件查询时会自动转换 不需要额外处理
	        Date createDateStart = DateUtil.string2Date(today + " 00:00:00", DateUtil.YYYYMMDDHHMMSS);
	        Date createDateEnd = DateUtil.string2Date(today + " 23:59:59", DateUtil.YYYYMMDDHHMMSS);
	 
	        query.addCriteria(Criteria.where("createDate").gte(createDateStart).lte(createDateEnd));
	        query.addCriteria(Criteria.where("spCode").is(param.getSpCode()));
	                
	        query.limit(param.getCount());
	        query.with(new Sort(Sort.Direction.DESC, "createDate"));
	        List<AlarmInfo> list = mongoTemplate.find(query, AlarmInfo.class, "alarm_info");
	        return list;
	    }
	 
	    /**
	     * 先将日期转化成 yyyy-mm-dd格式, 再用日期分组count
	     */
	    @Override
	    public List<CommunitySecurityStatis> communitySecurityStatis(CommunitySecurityParam param) {
	        logger.info("AlarmMongoDaoImpl.communitySecurityStatis start");
	        String begin = DateUtil.date2string(param.getBeginDate()).substring(0,10);
	        String end = DateUtil.date2string(param.getEndDate()).substring(0,10);
	        // 查询时间  mongoDB存储的是UTC时间 , 但读取和条件查询时会自动转换 不需要额外处理
	        Date createDateStart = DateUtil.string2Date(begin+" 00:00:00", DateUtil.YYYYMMDDHHMMSS);
	        Date createDateEnd = DateUtil.string2Date(end+" 23:59:59", DateUtil.YYYYMMDDHHMMSS);    
	        List<CommunitySecurityStatis> stats = new ArrayList<CommunitySecurityStatis>();
	        if (CollectionUtils.isNotEmpty(param.getRuleIds())) {
	            for (Long ruleId : param.getRuleIds()) {
	                
	                String ruleName = getRuleName(ruleId);
	                Criteria criteria = Criteria.where("createDate").gte(createDateStart).lte(createDateEnd);
	 
	                criteria.and("spCode").is(param.getSpCode());
	                criteria.and("ruleId").is(ruleId);
	                //聚合查询转换格式时需要增加8小时
	                Aggregation aggregation1 = Aggregation.newAggregation(Aggregation.match(criteria),
	                        Aggregation.project().andExpression("{$dateToString:{format:'%Y-%m-%d',date: {$add:{'$createDate',8*60*60000}}}}").as("date"),
	                        Aggregation.group("date").first("date").as("date")
	                        .count().as("num"));
	                AggregationResults<CommunitySecurityStatisData> outputTypeCount1 = mongoTemplate.aggregate(aggregation1, "alarm_info",
	                        CommunitySecurityStatisData.class);
	                if (null == outputTypeCount1 || CollectionUtils.isEmpty(outputTypeCount1.getMappedResults())) {
	                    continue;
	                }
	                
	                
	                CommunitySecurityStatis statis = new CommunitySecurityStatis();
	                List<CommunitySecurityStatisData> list = outputTypeCount1.getMappedResults();
	                statis.setName(ruleName);
	                statis.setRuleId(ruleId);
	                statis.setStatisData(list);
	                stats.add(statis);
	            }
	        }
	    
	        /*
	         * criteria.andOperator(Criteria.where("createTime").lte(param.getEndDate()),
	         * Criteria.where("createTime").gte(param.getBeginDate()));
	         */
	 
	 
	        
	 
	         
	        /*
	        GroupBy groupBy = GroupBy
	                .keyFunction("function(doc){ var date = new Date(doc.createTime);"
	                        + "    var dateKey = \"\"+date.getFullYear()+\"-\"+(date.getMonth()+1)+\"-\"+date.getDate();"
	                        + "    return {'date':dateKey};"
	                        + "}")
	                .initialDocument("{num : 0}").reduceFunction("function(doc, out){" 
	                        + "out.num += 1;" 
	                        + "}");
	        GroupBy groupBy1 = new GroupBy("{keyf : function(doc){ var date = new Date(doc.createTime);"
	                + "    var dateKey = \"\"+date.getFullYear()+\"-\"+(date.getMonth()+1)+\"-\"+date.getDate();"
	                + "    return {'date':dateKey};"
	                + "} }")
	                .initialDocument("{num : 0}")
	                        .reduceFunction("function(doc, out){" 
	                                + "out.num += 1;" 
	                                + "}");
	        GroupByResults<CommunitySecurityStatisData> statisResults = mongoTemplate.group(criteria, "alarm_info", groupBy1,
	                CommunitySecurityStatisData.class);
	        */
	        return stats;
	    }
	 
	    /**
	     * 单条件查询limit 1
	     * @param ruleId
	     * @return
	     */
	    private String getRuleName(Long ruleId) {
	        Query query = Query.query(Criteria.where("ruleId").is(ruleId));
	        AlarmInfo findOne = mongoTemplate.findOne(query, AlarmInfo.class);
	        if (null == findOne) {
	            return null;
	        }
	        return findOne.getRuleName();
	    }
	 
	    /*
	     * 以deviceTypeName分组 个数求和
	     */
	    @Override
	    public List<DeviceAlarmStatisDto> deviceAlarmStatis(Map<String, Object> paramMap) {
	 
	        Criteria criteria = Criteria.where("createDate").gte(paramMap.get("beginDate")).lte(paramMap.get("endDate"));
	 
	        if (StringUtils.isNotBlank((String) paramMap.get("spCode"))) {
	            criteria.and("spCode").is(paramMap.get("spCode"));
	        }
	 
	        // first("") 分组后，如果某属性(非分组属性)出现多个值 ,取第一个
	        Aggregation aggregation1 = Aggregation.newAggregation(Aggregation.match(criteria),
	                Aggregation.group("deviceTypeName").first("deviceTypeName").as("deviceTypeName").first("deviceType").as("deviceType").count().as("value"));
	        AggregationResults<DeviceAlarmStatisDto> outputTypeCount1 = mongoTemplate.aggregate(aggregation1, "alarm_info",
	                DeviceAlarmStatisDto.class);
	        if (null == outputTypeCount1) {
	            return null;
	        }
	 
	        return outputTypeCount1.getMappedResults();
	    }
	 
	    /*
	     * 分组统计  以statisDate分组并且将total值求和
	     */
	    @Override
	    public List<AlarmStatisDto> getAlarmStatis(Map<String, Object> paramMap) {
	        Date beginDate = (Date) paramMap.get("beginDate");
	        Date endDate = (Date) paramMap.get("endDate");
	        AlarmStatisParam paramDto = (AlarmStatisParam) paramMap.get("paramDto");
	        
	        Criteria criteria = Criteria.where("updateTime").gte(beginDate).lte(endDate);
	        if (StringUtils.isNotBlank(paramDto.getSpCode())) {
	            criteria.and("spCode").is(paramDto.getSpCode());
	        }
	        if (StringUtils.isNotBlank(paramDto.getProductCode())) {
	            criteria.and("produCode").is(paramDto.getProductCode());
	        }
	        if (StringUtils.isNotBlank(paramDto.getDeviceType())) {
	            criteria.and("deviceType").is(paramDto.getDeviceType());
	        }
	        if (StringUtils.isNotBlank(paramDto.getDeviceCode())) {
	            criteria.and("deviceCode").is(paramDto.getDeviceCode());
	        }
	        if (StringUtils.isNotBlank(paramDto.getDeviceGroupCode())) {
	            criteria.and("deviceGroupCodes").is(paramDto.getDeviceGroupCode());
	        }
	        
	        
	        Aggregation aggregation1 = Aggregation.newAggregation(Aggregation.match(criteria),
	                Aggregation.group("statisDate").first("statisDate").as("date").first("updateTime").as("updateTime").sum("total").as("value"),
	                Aggregation.sort(new Sort(Sort.Direction.ASC, "updateTime")));
	        String collectionName = "";
	        //按天
	        if (1 == (Integer)paramMap.get("type")) {
	            collectionName = "alarm_statis_per_day";
	        //按小时
	        }else if(0 == (Integer)paramMap.get("type")){
	            collectionName = "alarm_statis_per_hour";
	        }
	        
	        AggregationResults<AlarmStatisDto> outputTypeCount1 = mongoTemplate.aggregate(aggregation1, collectionName,
	                AlarmStatisDto.class);
	        List<AlarmStatisDto> mappedResults = outputTypeCount1.getMappedResults();
	 
	        // 分组函数 同样可以实现上面分组求和功能
	        /*GroupBy groupBy = new GroupBy("statisDate").
	                initialDocument("{date : '', value : 0}").
	                reduceFunction("function(doc, prev){"
	                        + "prev.date = doc.statisDate;"
	                        + "prev.value += doc.total;"
	                        + "}");
	        GroupByResults<AlarmStatisDto> statisResults = mongoTemplate.group(criteria, "alarm_statis_per_day", groupBy, AlarmStatisDto.class);
	        Iterator<AlarmStatisDto> iterator = statisResults.iterator();*/
	        return mappedResults;
	    }
	 
	    @Override
	    public AlarmStatisDto getAlarmStatisParam(AlarmStatisParam param) {
	           logger.info("getAlarmStatisToday start");
	 
	            Criteria criteria = Criteria.where("createDate").gte(param.getBegin()).lte(param.getEnd());
	            if (StringUtils.isNotBlank(param.getSpCode())) {
	                criteria.and("spCode").is(param.getSpCode());
	            }
	            if (StringUtils.isNotBlank(param.getProductCode())) {
	                criteria.and("produCode").is(param.getProductCode());
	            }
	            if (StringUtils.isNotBlank(param.getDeviceType())) {
	                criteria.and("deviceType").is(param.getDeviceType());
	            }
	            if (StringUtils.isNotBlank(param.getDeviceCode())) {
	                criteria.and("deviceCode").is(param.getDeviceCode());
	            }
	            if (StringUtils.isNotBlank(param.getDeviceGroupCode())) {
	                criteria .and("deviceGroupCodes").is(param.getDeviceGroupCode());
	            }
	 
	            Query query = Query.query(criteria);
	            int count = (int) mongoTemplate.count(query, AlarmInfo.class);
	            AlarmStatisDto dto = new AlarmStatisDto();
	            if (param.getStatisType() == 1) {
	                dto.setDate(DateUtil.date2string(param.getEnd(), DateUtil.YYYYMMDD));
	            }else {
	                dto.setDate(DateUtil.date2string(param.getEnd(), DateUtil.YYYYMMDDHH).substring(0,13)+":00:00");
	            }
	            dto.setUpdateTime(new Date());
	            dto.setValue(count);
	            return dto; 
	    }
	 
	}