# 方法1
用`Math.round`计算,这里返回的数字格式的。

	float price=89.89;
	int itemNum=3;
	float totalPrice=price*itemNum;
	float num=(float)(Math.round(totalPrice*100)/100);//如果要求精确4位就*10000然后/10000

# 方法2
用`DecimalFormat` 返回的是String格式的.该类对十进制进行全面的封装。像%号,千分位，小数精度，科学计算。

	float price=1.2;
	DecimalFormat decimalFormat=new DecimalFormat(".00");//构造方法的字符格式这里如果小数不足2位,会以0补足.
	String p=decimalFomat.format(price);//format 返回的是字符串

个人觉得在前台显示金额方面的还是用第二种方式。理由很简单是字符串格式的。