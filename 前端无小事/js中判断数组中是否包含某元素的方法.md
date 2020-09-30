> 原文链接：<https://www.cnblogs.com/yunshangwuyou/p/9032560.html>

这个是整理过的：<https://www.cnblogs.com/yunshangwuyou/p/10539090.html>

# 方法一
`arr.indexOf(某元素)`：未找到则返回`-1`。

实际用法：`if(arr.indexOf(某元素) > -1){//则包含该元素}`

**例：**

	var fruits = ["Banana", "Orange", "Apple", "Mango"];
	var a = fruits.indexOf("Apple"); // 2
	//以上输出结果意味着 "Apple" 元素位于数组中下标为 2 的位置。

`indexOf()`完整语法：`array.indexOf(item,start)`

**参数：**

- item：必须。查找的元素。
- start：可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是`0`到`stringObject.length - 1`。如省略该参数，则将从字符串的首字符开始检索。

**例：**

	var fruits=["Banana","Orange","Apple","Mango","Banana","Orange","Apple"];
	var a = fruits.indexOf("Apple",4); // 6

**注：**`string.indexOf()`返回某个指定的字符串值在字符串中首次出现的位置。

1. 该方法将从头到尾地检索字符串`stringObject`，看它是否含有子串`searchvalue`。开始检索的位置在字符串的 `fromindex`处或字符串的开头（没有指定`fromindex`时）。如果找到一个`searchvalue`，则返回`searchvalue`的第一次出现的位置。
2. `stringObject`中的字符位置是从`0`开始的。
3. 查找字符串最后出现的位置，使用`lastIndexOf()`方法。

# 方法二
数组实例的`arr.find()`方法用于找出第一个符合条件的数组元素。它的参数是一个回调函数，所有数组元素依次遍历该回调函数，直到找出第一个返回值为`true`的元素，然后返回该元素，否则返回`undefined`。 

`find()`方法为数组中的每个元素都调用一次函数执行：

- 当数组中的元素在测试条件时返回`true`时, `find()`返回符合条件的元素，之后的值不会再调用执行函数
- 如果没有符合条件的元素返回`undefined`

**注意:** 

- `find()`对于空数组，函数是不会执行的。 
- `find()` 并没有改变数组的原始值。


	[1, 5, 10, 15].find(function(value, index, arr) {
	    return value > 9;
	}) // 10

**实际用法**：

	arr.find(function(value) {
	    if(value === 要查找的值) {  
	        //则包含该元素
	    }
	})

# 方法三
`array.findIndex()`和`array.find()`十分类似，返回第一个符合条件的数组元素的位置，如果所有元素都不符合条件，则返回`-1`。 

`findIndex()`方法为数组中的每个元素都调用一次函数执行：当数组中的元素在测试条件时返回`true`时, `findIndex()`返回符合条件的元素的索引位置，之后的值不会再调用执行函数。 如果没有符合条件的元素返回`-1`。

**注意:** 

- `findIndex()`对于空数组，函数是不会执行的。 
- `findIndex()`并没有改变数组的原始值。

	[1,5,10,15].findIndex(function(value, index, arr) {
	    return value > 9;
	}) // 2.  
	 
	// 方法二和方法三，这两个方法都可以发现NaN，弥补了方法一IndexOf()的不足。
	 
	[NaN].indexOf(NaN) 
	//-1
	 
	[NaN].findIndex(y => Object.is(NaN, y))
	// 0

# 方法四
`for()`遍历数组，然后`if`判断。

	var arr = [1, 5, 10, 15];
	//传统for
	for(let i=0; i<arr.length; i++) {
	    if(arr[i] === 查找值) {
	        //则包含该元素
	    }
	}
	// for...of
	for(v of arr) {
	    if(v === 查找值) {
	        //则包含该元素
	    }
	}
	forEach。
	
	arr.forEach(v=>{
	   if(v === 查找值) {
	        //则包含该元素
	    }
	})

# 方法五
使用`jquery`的`inArray`方法，该方法返回元素在数组中的下标，如果不存在与数组中，那么返回`-1`，代码如下所示：

	// 使用jquery的inArray方法判断元素是否存在于数组中
	// @param {Object} arr 数组
	// @param {Object} value 元素值
	 
	function isInArray2(arr,value){
	    var index = $.inArray(value,arr);
	    if(index >= 0){
	        return true;
	    }
	    return false;
	}