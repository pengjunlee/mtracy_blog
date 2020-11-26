	import pandas as pd
	 
	list=[[1,2,3],[4,5,6],[7,8,9]]
	 
	column=['column1','column2','column3'] # 列表对应每列的列名
	 
	test=pd.DataFrame(columns=column,data=list)
	 
	test.to_csv('D:/test.csv') # 如果生成excel，可以用to_excel

<font color=red>注意：list里的字段值可以用for循环实现。</font>