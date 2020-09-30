> 官方文档地址：<http://bootstrap-table.wenzhixin.net.cn/zh-cn/documentation/>

# 引入BootStrap Table
引入`bootstrap.css`和`bootstrap-table.css`到`<head>`标签下。

	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
	<link rel="stylesheet" href="bootstrap.min.css">
	<link rel="stylesheet" href="bootstrap-table.min.css">
	</head>

引入`jquery.js` 、`bootstrap.js`和`bootstrap-table.js`到`<head>`标签下或者在`<body>`标签关闭之前（推荐）。 

	<body>
	//页面内容
	<script src="jquery.min.js"></script>
	<script src="bootstrap.min.js"></script>
	<script src="bootstrap-table.min.js"></script>
	<script src="bootstrap-table-zh-CN.min.js"></script>
	</body>

# 表格参数
表格的参数定义在`jQuery.fn.bootstrapTable.defaults`。 
<table border="1" cellpadding="0" cellspacing="0" style="width:622px;"><tbody><tr><td style="text-align:center;width:149px;"><span style="color:#3333ff;">名称</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#3333ff;">标签</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#3333ff;">类型</span></td>
			<td style="text-align:center;width:90px;"><span style="color:#3333ff;">默认</span></td>
			<td style="text-align:center;width:205px;"><span style="color:#3333ff;">描述</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">无</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-toggle</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'table'</span></td>
			<td style="width:205px;"><span style="color:#000000;">不用写 </span><span style="color:#009900;">JavaScript </span><span style="color:#000000;">直接启用表格</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">classes</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-classes</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'table table-hover'</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置表格的 </span><span style="color:#990000;">class </span><span style="color:#000000;">属性。<br>
			'</span><span style="color:#ff0000;">table-bordered</span><span style="color:#000000;">' ：有边框（默认）&nbsp;&nbsp;<br>
			'</span><span style="color:#ff0000;">table-no-bordered</span><span style="color:#000000;">' ：无边框</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">sortClass</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-sort-class</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">被排序的td元素的类名</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">height</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-height</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Number</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">定义表格的高度</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">undefinedText</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-undefined-text</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'-'</span></td>
			<td style="width:205px;"><span style="color:#000000;">当数据为 </span><span style="color:#990000;">undefined </span><span style="color:#000000;">时显示的字符</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">striped</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-striped</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true&nbsp;会有隔行变色效果</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">sortName</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-sort-name</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">定义排序列，通过url方式获取数据填写字段名，否则填写下标</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">sortOrder</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-sort-order</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'asc'</span></td>
			<td style="width:205px;"><span style="color:#000000;">定义排序方式，'</span><span style="color:#ff0000;">asc</span><span style="color:#000000;">' 或者 '</span><span style="color:#ff0000;">desc</span><span style="color:#000000;">'</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">sortStable</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-sort-stable</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true&nbsp;将获得稳定的排序，我们会添加\_position属性到 row 数据中</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">iconsPrefix</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-icons-prefix</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'glyphicon'</span></td>
			<td style="width:205px;"><span style="color:#000000;">定义字体库 ('</span><span style="color:#ff0000;">Glyphicon</span><span style="color:#000000;">' or '</span><span style="color:#ff0000;">fa</span><span style="color:#000000;">' for </span><span style="color:#ff0000;">FontAwesome</span><span style="color:#000000;">)，使用"</span><span style="color:#ff0000;">fa</span><span style="color:#000000;">"时需引用 </span><span style="color:#ff0000;">FontAwesome</span><span style="color:#000000;">，并且配合 icons 属性实现效果。<br>
			Glyphicon 集成于Bootstrap可免费使用，参考：&nbsp;<a href="http://glyphicons.com/" rel="nofollow">http://glyphicons.com/</a><br>
			FontAwesome 参考：&nbsp;<a href="http://fortawesome.github.io/" rel="nofollow">http://fortawesome.github.io/</a></span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">icons</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-icons</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Object</span></td>
			<td style="width:90px;"><span style="color:#000000;">{<br>
			&nbsp;&nbsp;paginationSwitchDown: 'glyphicon-collapse-down icon-chevron-down',<br>
			&nbsp;&nbsp;paginationSwitchUp: 'glyphicon-collapse-up icon-chevron-up',<br>
			&nbsp;&nbsp;refresh: 'glyphicon-refresh icon-refresh'<br>
			&nbsp;&nbsp;toggle: 'glyphicon-list-alt icon-list-alt'<br>
			&nbsp;&nbsp;columns: 'glyphicon-th icon-th'<br>
			&nbsp;&nbsp;detailOpen: 'glyphicon-plus icon-plus'<br>
			&nbsp;&nbsp;detailClose: 'glyphicon-minus icon-minus'<br>
			}</span></td>
			<td style="width:205px;"><span style="color:#000000;">自定义图标</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">columns</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">-</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Array</span></td>
			<td style="width:90px;"><span style="color:#000000;">[]</span></td>
			<td style="width:205px;"><span style="color:#000000;">列配置项，详情请查看 列参数 表格</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">data</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">-</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Array</span></td>
			<td style="width:90px;"><span style="color:#000000;">[]</span></td>
			<td style="width:205px;"><span style="color:#000000;">加载json格式的数据</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">ajax</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-ajax</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">自定义 AJAX 方法，须实现 jQuery AJAX API</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">method</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-method</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'get'</span></td>
			<td style="width:205px;"><span style="color:#000000;">服务器数据的请求方式 '</span><span style="color:#ff0000;">get</span><span style="color:#000000;">' 或 '</span><span style="color:#ff0000;">post</span><span style="color:#000000;">'</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">url</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-url</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">服务器数据的加载地址</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">cache</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-cache</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">true</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;false&nbsp;禁用 AJAX 数据缓存</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">contentType</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-content-type</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'application/json'</span></td>
			<td style="width:205px;"><span style="color:#000000;">发送到服务器的数据编码类型</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">dataType</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-data-type</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'json'</span></td>
			<td style="width:205px;"><span style="color:#000000;">服务器返回的数据类型</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">ajaxOptions</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-ajax-options</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Object</span></td>
			<td style="width:90px;"><span style="color:#000000;">{}</span></td>
			<td style="width:205px;"><span style="color:#000000;">提交ajax请求时的附加参数，可用参数列请查看<a href="http://api.jquery.com/jQuery.ajax" rel="nofollow">http://api.jquery.com/jQuery.ajax</a></span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">queryParams</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-query-params</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><span style="color:#000000;">function(params) {<br>
			return params;<br>
			}</span></td>
			<td style="width:205px;"><span style="color:#000000;">请求服务器数据时，你可以通过重写参数的方式添加一些额外的参数，例如 toolbar 中的参数 如果 queryParamsType = 'limit' ,返回参数必须包含</span><br><span style="color:#ff0000;">limit, offset, search, sort, order</span><span style="color:#000000;"> 否则, 需要包含:&nbsp;</span><br><span style="color:#ff0000;">pageSize, pageNumber, searchText, sortName, sortOrder.&nbsp;</span><br><span style="color:#000000;">返回false将会终止请求</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">queryParamsType</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-query-params-type</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'limit'</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为 '</span><span style="color:#ff0000;">limit</span><span style="color:#000000;">' 则会发送符合 RESTFul 格式的参数</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">responseHandler</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-response-handler</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><br><span style="color:#000000;">function(res) {<br>
			return res;<br>
			}</span></td>
			<td style="width:205px;"><span style="color:#000000;">加载服务器数据之前的处理程序，可以用来格式化数据。<br>
			参数：res为从服务器请求到的数据</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">pagination</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-pagination</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true&nbsp;会在表格底部显示分页条。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">paginationLoop</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-pagination-loop</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">true</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true&nbsp;启用分页条无限循环的功能。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">onlyInfoPagination</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-only-info-pagination</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true&nbsp;只显示总数据数，而不显示分页按钮。需要设置 pagination='true'。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">sidePagination</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-side-pagination</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'client'</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置在哪里进行分页，可选值为 'client' 或者 'server'。设置 'server'时，必须设置服务器数据地址（url）或者重写ajax方法。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">pageNumber</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-page-number</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Number</span></td>
			<td style="width:90px;"><span style="color:#000000;">1</span></td>
			<td style="width:205px;"><span style="color:#000000;">如果设置了分页，首页页码。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">pageSize</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-page-size</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Number</span></td>
			<td style="width:90px;"><span style="color:#000000;">10</span></td>
			<td style="width:205px;"><span style="color:#000000;">如果设置了分页，页面数据条数。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">pageList</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-page-list</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Array</span></td>
			<td style="width:90px;"><span style="color:#000000;">[10, 25, 50, 100, All]</span></td>
			<td style="width:205px;"><span style="color:#000000;">如果设置了分页，设置可供选择的页面数据条数。设置为 All 或者 Unlimited，则显示所有记录。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">selectItemName</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-select-item-name</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'btSelectItem'</span></td>
			<td style="width:205px;"><span style="color:#000000;">radio 或者 checkbox 的字段 name 名。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">smartDisplay</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-smart-display</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">true</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为 true 是程序自动判断显示分页信息和 card 视图。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">escape</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-escape</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">转义HTML字符串，替换&nbsp;&amp;,&nbsp;&lt;,&nbsp;&gt;,&nbsp;",&nbsp;\`, 和&nbsp;'&nbsp;字符。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">search</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-search</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">是否启用搜索框。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">searchOnEnterKey</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-search-on-enter-key</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true时，按回车触发搜索方法，否则自动触发搜索方法。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">strictSearch</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-strict-search</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true启用全匹配搜索，否则为模糊搜索。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">searchText</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-search-text</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">''</span></td>
			<td style="width:205px;"><span style="color:#000000;">初始化搜索文字。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">searchTimeOut</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-search-time-out</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Number</span></td>
			<td style="width:90px;"><span style="color:#000000;">500</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置搜索超时时间。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">trimOnSearch</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-trim-on-search</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">true</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true&nbsp;将自动去掉搜索字符的前后空格。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">showHeader</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-show-header</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">true</span></td>
			<td style="width:205px;"><span style="color:#000000;">是否显示列头。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">showFooter</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-show-footer</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">是否显示列脚。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">showColumns</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-show-columns</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">是否显示内容列下拉框。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">showRefresh</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-show-refresh</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">是否显示刷新按钮。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">showToggle</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-show-toggle</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">是否显示切换视图（table/card）按钮。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">showPaginationSwitch</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-show-pagination-switch</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">是否显示切换分页按钮。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">showFullscreen</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-show-fullscreen</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">是否显示全屏按钮。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">minimumCountColumns</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-minimum-count-columns</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Number</span></td>
			<td style="width:90px;"><span style="color:#000000;">1</span></td>
			<td style="width:205px;"><span style="color:#000000;">最小隐藏列的数量。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">idField</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-id-field</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定主键列。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">uniqueId</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-unique-id</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">对每一行指定唯一标识符。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">cardView</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-card-view</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true将显示card视图，适用于移动设备。否则为table试图，适用于pc端。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">detailView</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-detail-view</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true&nbsp;可以显示详细页面模式。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">detailFormatter</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-detail-formatter</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><span style="color:#000000;">function(index, row) {<br>
			return '';<br>
			}</span></td>
			<td style="width:205px;"><span style="color:#000000;">格式化详细页面模式的视图。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">searchAlign</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-search-align</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'right'</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定 搜索框 水平方向的位置。'left' 或 'right'。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">buttonsAlign</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-buttons-align</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'right'</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定 按钮栏 水平方向的位置。'left' 或 'right'。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">toolbarAlign</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-toolbar-align</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'left'</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定 toolbar 水平方向的位置。'left' 或 'right'。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">paginationVAlign</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-pagination-v-align</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'bottom'</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定 分页条 在垂直方向的位置。'top'，'bottom' 或 'both'。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">paginationHAlign</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-pagination-h-align</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'right'</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定 分页条 在水平方向的位置。'left' 或 'right'。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">paginationDetailHAlign</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-pagination-detail-h-align</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'left'</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定 分页详细信息 在水平方向的位置。'left' 或 'right'。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">paginationPreText</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-pagination-pre-text</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'&lt;'</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定分页条中上一页按钮的图标或文字。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">paginationNextText</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-pagination-next-text</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">'&gt;'</span></td>
			<td style="width:205px;"><span style="color:#000000;">指定分页条中下一页按钮的图标或文字。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">clickToSelect</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-click-to-select</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置 true 将在点击行时，自动选择 rediobox 和 checkbox。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">ignoreClickToSelectOn</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-ignore-click-to-select-on</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><span style="color:#000000;">{ return $.inArray(element.tagName, ['A', 'BUTTON']); }</span></td>
			<td style="width:205px;"><span style="color:#000000;">包含一个参数：<br>
			element: 点击的元素。<br>
			返回 true 是点击事件会被忽略，返回 false 将会自动选中。该选项只有在 clickToSelect 为 true 时才生效。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">singleSelect</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-single-select</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置 true 将禁止多选。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">toolbar</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-toolbar</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">一个jQuery 选择器，指明自定义的 toolbar。例如:<br>
			#toolbar, .toolbar.</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">buttonsToolbar</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-buttons-toolbar</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">String | Node</span></td>
			<td style="width:90px;"><span style="color:#000000;">undefined</span></td>
			<td style="width:205px;"><span style="color:#000000;">一个jQuery 选择器，指明自定义的 buttons toolbar。例如:<br>
			&nbsp;&nbsp;&nbsp;&nbsp;#buttons-toolbar, .buttons-toolbar 或 DOM 节点。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">checkboxHeader</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-checkbox-header</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">true</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置 false 将在列头隐藏全选复选框。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">maintainSelected</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-maintain-selected</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">false</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;true&nbsp;在点击分页按钮或搜索按钮时，将记住checkbox的选择项。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">sortable</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-sortable</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">true</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为false&nbsp;将禁止所有列的排序。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">silentSort</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-silent-sort</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Boolean</span></td>
			<td style="width:90px;"><span style="color:#000000;">true</span></td>
			<td style="width:205px;"><span style="color:#000000;">设置为&nbsp;false&nbsp;将在点击分页按钮时，自动记住排序项。仅在 sidePagination设置为&nbsp;server时生效。</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">rowStyle</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-row-style</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><span style="color:#000000;">function(row,index) {<br>
			return class;<br>
			}</span></td>
			<td style="width:205px;"><span style="color:#000000;">自定义行样式 参数为：<br>
			row: 行数据<br>
			index: 行下标<br>
			返回值可以为class或者css</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">rowAttributes</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-row-attributes</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><span style="color:#000000;">function(row,index) {<br>
			return attributes;<br>
			}</span></td>
			<td style="width:205px;"><span style="color:#000000;">自定义行属性 参数为：<br>
			row: 行数据<br>
			index: 行下标<br>
			返回值可以为class或者css 支持所有自定义属性</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">customSearch</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-custom-search</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><span style="color:#000000;">$.noop</span></td>
			<td style="width:205px;"><span style="color:#000000;">自定义搜索方法来替代内置的搜索功能，它包含一个参数：</span><br><span style="color:#000000;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;text：搜索文字。</span><br><span style="color:#000000;">用法示例：</span><br><span style="color:#006600;">function customSearch(text) {<br>
			//Search logic here.<br>
			//You must use `this.data` array in order to filter the data. NO use `this.options.data`.<br>
			}</span></td>
		</tr><tr><td style="text-align:center;width:149px;"><span style="color:#000000;">customSort</span></td>
			<td style="text-align:center;width:108px;"><span style="color:#000000;">data-custom-sort</span></td>
			<td style="text-align:center;width:70px;"><span style="color:#000000;">Function</span></td>
			<td style="width:90px;"><br><span style="color:#000000;">$.noop</span></td>
			<td style="width:205px;"><span style="color:#000000;">自定义排序方法来替代内置的排序功能，它包含一个参数：</span><br><span style="color:#000000;">sortName: 排序名。</span><br><span style="color:#000000;">sortOrder: 排序顺序。</span><br><span style="color:#000000;">用法示例：</span><br><span style="color:#006600;">function customSort(sortName, sortOrder) {<br>
			//Sort logic here.<br>
			//You must use `this.data` array in order to sort the data. NO use `this.options.data`.<br>
			}</span></td>
		</tr></tbody></table>

# 应用示例
无需编写`JavaScript`启用`bootstrap table`，仅需对普通的`table`设置`data-toggle="table"`即可。 

	<table data-toggle="table" data-classes="table table-bordered table-hover" data-striped="true"> 
	  <thead> 
	    <tr> 
	      <th>员工编号</th>  
	      <th>员工姓名</th>  
	      <th>所在部门</th>  
	      <th>武功等级</th> 
	    </tr> 
	  </thead>  
	  <tbody> 
	    <tr> 
	      <td>89757</td>  
	      <td>姬如雪</td>  
	      <td>幻音坊</td>  
	      <td>24</td> 
	    </tr>  
	    <tr> 
	      <td>89756</td>  
	      <td>叶星云</td>  
	      <td>天元神宗</td>  
	      <td>31</td> 
	    </tr>  
	    <tr> 
	      <td>89755</td>  
	      <td>唐三</td>  
	      <td>史莱克学院</td>  
	      <td>33</td> 
	    </tr> 
	  </tbody> 
	</table>

此外也可以通过设置远程的`url`如`data-url='返回json格式内容的url'`来加载数据。 

	<table data-toggle="table" data-classes="table table-bordered table-hover" data-url="http://localhost:8080/employee/list" data-striped="true" data-sort-name="id" data-sort-order="desc"> 
	  <thead> 
	    <tr> 
	      <th data-field="id">员工编号</th>  
	      <th data-field="name">员工姓名</th>  
	      <th data-field="deptName">所在部门</th>  
	      <th data-field="level">武功等级</th> 
	    </tr> 
	  </thead> 
	</table>

在`JavaScript`中通过`<table>`的`id`属性来启用`bootstrap table`。 

	<table id="example_table"/>
	$('#example_table').bootstrapTable({
	    data: [{
	        "id": 89757,
	        "name": "姬如雪",
	        "deptName": "幻音坊",
	        "level": 24
	    },
	    {
	        "id": 89756,
	        "name": "叶星云",
	        "deptName": "天元神宗",
	        "level": 31
	    },
	    {
	        "id": 89755,
	        "name": "唐三",
	        "deptName": "史莱克学院",
	        "level": 33
	    }],
	    columns: [{
	        field: 'id',
	        title: '员工编号'
	    },
	    {
	        field: 'name',
	        title: '员工姓名'
	    },
	    {
	        field: 'deptName',
	        title: '所属部门'
	    },
	    {
	        field: 'level',
	        title: '武功等级'
	    }]
	 
	});

同样地，在`JavaScript`中也可以通过设置远程的`url`如`url: '返回json格式内容的url'`来加载数据。  

	$('#example_table').bootstrapTable({
	    url: 'http://localhost:8080/employee/list',
	    method: 'get',
	    contentType: 'application/json',
	    dataType: 'json',
	    columns: [{
	        field: 'id',
	        title: '员工编号'
	    },
	    {
	        field: 'name',
	        title: '员工姓名'
	    },
	    {
	        field: 'deptName',
	        title: '所属部门'
	    },
	    {
	        field: 'level',
	        title: '武功等级'
	    }]
	});

各项参数示例：  

	$(function() {
	    $('#example_table').bootstrapTable({
	        //设置表格的 class 属性
	        classes: 'table table-bordered table-hover',
	        //设置表格的高度
	        height: 500,
	        //设置当数据为 undefined 时显示的字符
	        undefinedText: '空',
	        //设置表格隔行变色效果
	        striped: true,
	        //设置表格使用的字体库
	        iconsPrefix: 'glyphicon',
	        //自定义表格中的图标
	        icons: {
	            paginationSwitchDown: 'glyphicon-collapse-down icon-chevron-down',
	            paginationSwitchUp: 'glyphicon-collapse-up icon-chevron-up',
	            refresh: 'glyphicon-refresh icon-refresh',
	            toggle: 'glyphicon-list-alt icon-list-alt',
	            columns: 'glyphicon-th icon-th',
	            detailOpen: 'glyphicon-plus icon-plus',
	            detailClose: 'glyphicon-minus icon-minus'
	        },
	        //设置数据的加载地址
	        url: 'http://localhost:8080/employee/list',
	        //设置数据的请求方式
	        method: 'get',
	        //设置发送到服务器的数据编码类型
	        contentType: 'application/json',
	        //设置服务器返回的数据类型
	        dataType: 'json',
	        //发送符合 RESTFul 格式的参数
	        queryParamsType: 'limit',
	        //设置数据请求的额外参数
	        queryParams: function(params) {
	            var param = {};
	            param['offset'] = params.offset; // 页码
	            param['limit'] = params.limit; // 条数
	            param['search'] = '搜索内容'; // 搜索内容
	            param['sort'] = '排序字段'; // 排序字段
	            param['order'] = '排序方式'; // 排序方式
	            return param;
	        },
	        //对请求到的数据进行处理
	        responseHandler: function(res) {
	            for (var index in res) {
	                var id = res[index]['id'];
	                res[index]['id'] = 'No.' + id;
	            };
	            res[0]['deptName'] = undefined;
	            return res;
	        },
	        //对返回数据中的特殊字符进行转义
			escape: true,
			//自定义Ajax请求
	        //ajax : ajaxReqData(),
			//禁用Ajax数据缓存
	        //cache : false,
			//Ajax请求时的附加参数
	        //ajaxOptions : {},
	        //设置排序列
	        sortName: 'level',
	        //设置排序方式
	        sortOrder: 'desc',
	        //自定义Ajax请求
	        sortClass: 'sort_class',
	        //获得稳定的排序
	        sortStable: true,
	        //表格底部显示分页条
	        pagination: true,
	        //启用分页条无限循环的功能
	        paginationLoop: true,
	        //只显示总数据数
	        onlyInfoPagination: true,
	        //采用 client 分页模式
	        sidePagination: 'client',
	        //首页页码
	        pageNumber: 1,
	        //每页数据条数
	        pageSize: 2,
	        //设置可供选择的页面数据条数
	        pageList: [2, 5, 10],
	        //radio 或者 checkbox 的字段 name 名
	        selectItemName: 'btSelectItem',
	        //自动判断显示分页信息和 card 视图
	        smartDisplay: true,
	        //启用搜索框
	        search: true,
	        //按回车触发搜索方法
	        searchOnEnterKey: true,
	        //e启用全匹配搜索
	        strictSearch: true,
	        //初始化搜索文字
	        searchText: '',
	        //设置搜索超时时间
	        searchTimeOut: 500,
	        //自动去掉搜索字符的前后空格
	        trimOnSearch: true,
	        //显示列头
	        showHeader: true,
	        //显示列脚
	        showFooter: true,
	        //显示内容列下拉框
	        showColumns: true,
	        //显示刷新按钮
	        showRefresh: true,
	        //显示切换视图（table/card）按钮
	        showToggle: true,
	        //显示切换分页按钮
	        showPaginationSwitch: true,
	        //显示全屏按钮
	        showFullscreen: true,
	        //最小隐藏列的数量
	        minimumCountColumns: 1,
	        //指定主键列
	        idField: 'id',
	        //对每一行指定唯一标识符
	        uniqueId: 'id',
	        //使用table试图，适用于pc端
	        cardView: false,
	        //显示详细页面模式
	        detailView: true,
	        //格式化详细页面模式的视图
	        detailFormatter: function(index, row) {
	            return '<span style="color:blue;font-weight:bold">员工编号</span>:' + row.id + '，<span style="color:red;font-weight:bold">员工姓名：</span>' + row.name
	        },
	        //指定 搜索框 水平方向的位置
	        searchAlign: 'right',
	        //指定 按钮栏 水平方向的位置
	        buttonsAlign: 'right',
	        //指定 toolbar 水平方向的位置
	        toolbarAlign: 'right',
	        //指定 分页条 在垂直方向的位置
	        paginationVAlign: 'bottom',
	        //指定 分页条 在水平方向的位置
	        paginationHAlign: 'right',
	        //指定 分页详细信息 在水平方向的位置
	        paginationDetailHAlign: 'right',
	        //指定分页条中上一页按钮的图标或文字
	        paginationPreText: '<',
	        //指定分页条中下一页按钮的图标或文字
	        paginationNextText: '>',
	        //点击行时自动选择 rediobox 和 checkbox
	        clickToSelect: true,
	        //返回 true 时点击事件会被忽略
	        ignoreClickToSelectOn: function(ele) {
	            return $.inArray(ele.tagName, ['A', 'BUTTON']);
	        },
	        //禁止多选
	        singleSelect: true,
	        //自定义 toolbar
	        toolbar: '#my_toolbar',
	        //自定义 buttons toolbar
	        buttonsToolbar: '#my_buttonsToolbar',
	        //列头隐藏全选复选框
	        checkboxHeader: false,
	        //点击分页按钮或搜索按钮时，记住checkbox的选择项
	        maintainSelected: true,
	        //启用排序
	        sortable: true,
			//静默排序
	        //silentSort : true,
	        //自定义行样式
	        rowStyle: function(row, index) {
	            return {
	                css: {
	                    "color": "gray"
	                }
	            }
	        },
	        //自定义行属性
	        rowAttributes: function(row, index) {
	            return {
	                css: {
	                    "row_index": index
	                }
	            }
	        },
	        //自定义搜索方法
	        customSearch: function(text) {
	            console.log(text);
	        },
	        //自定义排序方法
	        customSort: function customSort(sortName, sortOrder) {
	            console.log(sortName);
	            console.log(sortOrder);
	        },
			//设置显示字段内容
	        columns: [{
	            field: 'id',
	            title: '员工编号'
	        },
	        {
	            field: 'name',
	            title: '员工姓名'
	        },
	        {
	            field: 'deptName',
	            title: '所属部门'
	        },
	        {
	            field: 'level',
	            title: '武功等级'
	        }]
	 
	    });
	});
	function ajaxReqData() {
	    $.ajax({
	        type: "GET",
	        url: "http://localhost:8080/employee/list",
	        contentType: "application/json;charset=utf-8",
	        dataType: "json",
	        data: "",
	        success: function(data) {
	            $('#example_table').bootstrapTable('load', data);
	        },
	        error: function(data) {
	            console.log("数据加载错误");
	        }
	    })
	}
  