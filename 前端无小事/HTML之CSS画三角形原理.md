在进行WEB应用开发的过程中，我们经常会需要使用到三角形图标，例如下面这个下拉选择控件右侧的收缩箭头。
<div align=center>

![三角形](./imgs/39.png "三角形示意图")
<div align=left>

又或者像下面这种情形。
<div align=center>

![三角形](./imgs/40.png "三角形示意图")
<div align=left>

搜索网络之后发现三角形可以通过以下CSS代码实现：

	#triangle_bottom{
		height:0px;
		width:0px;
		border-left:20px solid transparent;
		border-right:20px solid transparent ;
		border-bottom:20px solid #9E9E9E ;
	}

效果如下：
<div align=center>

![三角形](./imgs/41.png "三角形示意图")
<div align=left>

那么，其内部到底是如何实现的呢？接下来是我学习CSS画三角形原理的一点小总结。
其实使用CSS代码绘制三角形，只是对盒子模型中的`border`属性的简单应用。盒子模型将HTML元素划分为内容(`Content`)、填充(`Padding`)、边框(`Border`)和边界(`Margin`)四部分，参照下图。
<div align=center>

![三角形](./imgs/42.png "三角形示意图")
<div align=left>

为了能更好的看清`Border`的四条边界的真实形状，我们为`Border`的四条边设置上不同的颜色。

	border-left:red;
	border-top:blue;
	border-right:green;
	border-bottom:yellow;

<div align=center>

![三角形](./imgs/43.png "三角形示意图")
<div align=left>

不难看出，当`Border`的四条边宽度相同时，每条边均为等腰梯形。
为四条边设置各不相同的宽度，其各边的形状改变为如下图所示。

	border-left:20px red;
	border-top:10px blue;
	border-right:30px green;
	border-bottom:40px yellow;

<div align=center>

![三角形](./imgs/44.png "三角形示意图")
<div align=left>

删除底部一条边后，其相邻边界的形状改变如下。

	border-bottom:0px;

<div align=center>

![三角形](./imgs/45.png "三角形示意图")
<div align=left>

由此我们不难得出CSS画三角形所需的第一条结论：<font color=blue>每条边(以黄色边为例)与其邻边所成夹角A，tanA=n/m（n,m分别为自己和邻边的宽度），当邻边宽度为0px时，A角大小为90°</font>。
<div align=center>

![三角形](./imgs/46.png "三角形示意图")
<div align=left>

接下来我们将盒子模型中的内容(Content)和填充(Padding)都设置为0px，四条边的宽度相同时如下图所示。
<div align=center>

![三角形](./imgs/47.png "三角形示意图")
<div align=left>

四条边宽度不同时，各边形状如下图。
<div align=center>

![三角形](./imgs/48.png "三角形示意图")
<div align=left>

当`border-top`宽度为`0px`时，其它三边形状如下图。
<div align=center>

![三角形](./imgs/49.png "三角形示意图")
<div align=left>

再将上图中的左侧红色和右侧绿色三角形的颜色设置为透明(transparent)，其最终形状如下图。
<div align=center>

![三角形](./imgs/50.png "三角形示意图")
<div align=left>
由此我们可以得出CSS画三角形所需的第二条结论：当盒子模型中的内容(`Content`)+填充(`Padding`)的大小为`0px`时，`Border`边的形状将由梯形变为三角形。

有了以上两条结论，我们就可以通过控制`Border`各条边的宽度和设置透明色来轻松画出各种角度的三角形了。以画向上的底角45°的等腰三角形为例，由于tan(45°)=1，我们需将`border-bottom`、`border-left`、`border-right`三者设置为相等的宽度，并将`border-left`和`border-right`设置为透明色，代码如下：

	#triangle_bottom{
		height:0px;
		width:0px;
		border-left:20px solid transparent;
		border-right:20px solid transparent ;
		border-bottom:20px solid #FF9800;
	}

其画出的三角形效果如下：
<div align=center>

![三角形](./imgs/51.png "三角形示意图")
<div align=left>

其它三个朝向的三角形画法依此类推，四个朝向的三角形的完整代码如下：

	<!DOCTYPE HTML>
	<HTML>
	<head>
		<style type="text/css">
			.triangle_left{
			    width: 0;
			    height: 0;
			    border-left: 50px solid red;
			    border-top: 50px solid transparent;
			    border-bottom: 50px solid transparent;
			 
			}
			.triangle_top{
			    width: 0;
			    height: 0;
			    border-top: 50px solid blue;
			    border-right: 50px solid transparent;
			    border-left: 50px solid transparent;
			}
			.triangle_right{
			    width: 0;
			    height: 0;
			    border-right: 50px solid green;
			    border-top: 50px solid transparent;
			    border-bottom: 50px solid transparent;
			}
			.triangle_bottom{
			    width: 0;
			    height: 0;
			    border-bottom: 50px solid yellow;
			    border-right: 50px solid transparent;
			    border-left: 50px solid transparent;
			}
		</style>
	</head>
	<body>
		<div class="triangle_left"></div>
		<div class="triangle_top"></div>
		<div class="triangle_right"></div>
		<div class="triangle_bottom"></div>
	</body>
	</HTML>

效果如下：
<div align=center>

![三角形](./imgs/52.png "三角形示意图")
<div align=left>

