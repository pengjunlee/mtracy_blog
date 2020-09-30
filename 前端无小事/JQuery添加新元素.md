	<meta http-equiv="Content-Type" content="text/html;charset=utf-8">
	<title>第一个JQuery文档</title>
	<script src="jquery.js"></script>
	<script type="text/javascript">
	//使用JQuery操纵网页元素，实现新元素添加并绑定事件。
	$(document).ready(
	    function(e){
	        $("#msg").css("font-size","9px");         //更改DIV元素的字体  
	        $("#msg").click(
	            function(e){
	                alert($(this).html());      
	            }
	        );                        //为DIV元素添加CLICK响应        
	
	        $("<b>Hello World!</b>").appendTo("body"); //向页面BODY添加一个新的B元素         
	
	        $("<div>",{
	                    style:"width:200px;height:200px;background-color:silver;"
	                  }).hover(function(){ alert('鼠标移入！');},function(){alert('鼠标移出！'); }).appendTo("body");              //向页面BODY添加一个新的DIV元素,并添加JQuery的hover事件响应。
	
	        /*
	        *hover()是jQuery为了方便用户固定调用mouseenter和mouseleave事件而重新定义的内部事件，它并非一个真正的事件，不能使用bind()绑定。
	        *所以使用以下代码是无法实现Hover事件绑定效果的。
	        */
	        $("<div>",{
	                    style:"width:200px;height:200px;background-color:purple;"
	                  }).bind({hover:(function(){ alert('mouseover'); },function({alert('mouseout'); })}).appendTo("body");        
	
	        /*
	        *可以直接绑定mouseenter/mouseleave或mouseover/mouseout事件达到与Hover事件相同效果。
	        */
	        $("<div>",{style:"width:200px;height:200px;background-color:maroon;"
	        }).bind({
	            mouseover:(function(){ alert('mouseover'); })
	            }).bind({
	            mouseout:(function(){ alert('mouseout'); })
	        }).appendTo("body");
	          
	        $("<div>",{
	            style:"font-size:9px;background-color:navy;width:200px;height:200px;",
	            text:"单击这里改变颜色",
	            click:function(){
	            $(this).css("background-color","green");},
	            mouseenter:function(){
	            $(this).css("background-color","blue");},
	            mouseleave:function(){
	            $(this).css("background-color","yellow");}
	        }).appendTo("body");         
	
	        $("<div>",{style:"width:200px;height:200px;background-color:olive;"
	        }).bind({                
	            click:(function(){ alert('mouseclick'); })
	        }).appendTo("body");     //向页面BODY添加一个新的DIV元素,并添加CLICK响应。
	    }
	);
	
	</script> 
	
	<div id="msg" style="width:200px;height:200px;background-color:red"><span>第一个JQuery文档</span></div>
