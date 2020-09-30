# jQuery Validate简介
`jQuery Validate`插件提供了强大的表单验证功能，能够让客户端表单验证变得更简单，同时它还提供了大量的可定制化选项，以满足应用程序的各种需求。该插件捆绑了一套非常有用的验证方法，包括 URL 和电子邮件验证，同时也提供了`API`允许用户自定义校验方法。提供的所有捆绑方法默认使用英语作为错误信息，且已翻译成其他`37`种语言。

# 引入jQuery Validate
官网下载地址：<https://github.com/jquery-validation/jquery-validation/releases>
根据项目需要引入需要的Js库文件到`<head>`标签下或者在`<body>`标签关闭之前（推荐）。

	<body>
	   //页面内容 
	  <script src="jquery.min.js"></script> 
	  <script src="jquery.validate.min.js"></script> 
	  <script src="additional-methods.min.js"></script> 
	  <script src="messages_zh.min.js"></script>
	</body>

<font color=red>注意事项</font>：

1. 所有需要被校验的`<input>`元素都必须有`name`属性，并且其取值在一个表单中必须唯一。
2. 同组的`<radio>`或`<checkbox>`元素`name`属性值相同。、
3. 复杂的`name`属性在定义`rules`选项时需要使用`""`括起来。
4. 推荐为每一个`<input>`元素添加一个与其关联的`<label>`元素，`<label>`元素的`for`属性为`<input>`元素的 `id`属性。

	$("#example_form").validate({
	    rules: {
	        // 不需要使用 "" 括起来
	        name: "required",
	        // 需要使用 "" 括起来
	        "user[email]": "email",
	        // 需要使用 "" 括起来
	        "user.address.street": "required"
	    }
	});

# 默认校验规则
<table cellspacing="0" cellpadding="0" border="1" style="table-layout:fixed;border:1px solid rgb(204,204,204);width:370px;"><tbody><tr><td style="width:80px;font-weight:bold;text-align:center;color:rgb(84,141,212);"><span style="font-family:'Microsoft YaHei';font-size:12px;">规则名称</span></td><td style="width:98px;height:12px;font-weight:bold;text-align:center;color:rgb(84,141,212);"><span style="font-family:'Microsoft YaHei';font-size:12px;">类型</span></td><td style="width:192px;height:12px;font-weight:bold;text-align:center;color:rgb(84,141,212);"><span style="font-family:'Microsoft YaHei';font-size:12px;">描述</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">required</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为必填</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">remote</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Json|String</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">请求远程资源来校验内容有效性</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">minlength</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Number</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置内容的最少字符长度</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">maxlength</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Number</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置内容的最多字符长度</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">rangelength</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Array</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置内容的字符长度范围</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">min</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Number</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置内容的最小允许值</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">max</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Number</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置内容的最大允许值</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">range</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Array</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置内容的允许值范围</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">step</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Number</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置内容为某一固定值的倍数</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">email</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为一个有效邮箱地址</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">url</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为一个有效网址</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">date</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为日期格式</span></td></tr><tr><td style="width:80px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">dateISO</span></td><td style="width:98px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为ISO日期格式</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">number</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为十进制小数</span></td></tr><tr><td style="width:80px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">digits</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为十进制整数</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">equalTo</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Selector</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容与指定元素内容相同</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">accept</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">String</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置上传文件所接受的 MIME 类型</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">creditcard</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为一个信用卡卡号</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">extension</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">String</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置上传文件所接受的扩展名</span></td></tr><tr><td style="width:80px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">phoneUS</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Boolean</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置该项内容为一个美国电话号码</span></td></tr><tr><td style="width:80px;text-align:center;"><span style="font-family:'Microsoft YaHei';font-size:12px;">require_from_group</span></td><td style="width:98px;height:12px;text-align:center;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">Array</span></td><td style="width:192px;height:12px;color:rgb(0,0,0);"><span style="font-family:'Microsoft YaHei';font-size:12px;">设置同一组至少填写多少项</span></td></tr></tbody></table>

# 插件功能
## 选择器
`jQuery Validate`插件对`JQuery`库进行了扩展，增加了下面3个选择器：

	:blank –选择所有值为空的元素
	:filled – 选择所有值不为空的元素
	:unchecked –选择所有未被选中的元素

## 方法
`jQuery Validate`插件提供了3个非常重要的校验方法：

- validate() – 对选中的表单进行校验
- valid() – 判断选中的表单或元素输入的内容是否有效
- rules() – 获取、添加或者移除元素的规则

**方法应用示例**：

	// 移除元素的所有校验规则
	$("#user_age").rules("remove");
	// 为元素添加 required max min 校验规则
	$("#user_age").rules("add", {
	    required: true,
	    max: 100,
	    min: 10
	});
	// 移除元素的 max min 校验规则
	$("#user_age").rules("remove", "min max");

## Validator
调用`validate()`方法将会返回一个`Validator`对象，该对象提供了很多方法，其中比较常用的方法列举如下。

- Validator.form() – 校验表单
- Validator.element() – 校验元素
- Validator.resetForm() – 重置表单
- Validator.showErrors() – 显示指定的错误信息
- Validator.numberOfInvalids() – 显示无效的项目数量
- Validator.destroy() –销毁Validator对象

**方法应用示例**：

	var validator = $("#example_form").validate();
	validator.element("#user_age");
	validator.showErrors({
	    "userAge": "年龄输入格式不合法"
	});
	validator.form();
	console.log(validator.numberOfInvalids() + " 个字段无效");
	validator.resetForm();
	validator.destroy();

## 静态方法
同时，`Validator`对象还提供了下面几个静态方法。

- jQuery.validator.addMethod() – 添加一个自定义的校验方法
- jQuery.validator.format() – 使用参数替换掉 {n} 占位符
- jQuery.validator.setDefaults() – 修改校验器的默认选项
- jQuery.validator.addClassRules() – 为某一类元素添加校验规则

# 应用示例
<div align=center>

![flex](./imgs/73.png "flex示意图")
<div align=left>
对以上注册信息进行验证，完整代码如下。

	<!DOCTYPE html>
	<html>
	 <head>
	  <meta charset="UTF-8" />  
	  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" /> 
	 </head> 
	 <body> 
	  <form id="example_form" method="get" action=""> 
	   <fieldset> 
	    <legend>填写注册信息</legend> 
	    <p> <label for="user_name">登录名</label> <input id="user_name" name="name" /> </p> 
	    <p> <label for="user_age">年龄</label> <input class="math_class" id="user_age" name="age" /> </p> 
	    <p> <label for="user_birthday">生日</label> <input id="user_birthday" name="birthday" /> </p> 
	    <p> <label for="user_card_id">信用卡号</label> <input id="user_card_id" name="card" /> </p> 
	    <p> <label for="user_salary">月薪</label> <input id="user_salary" name="salary" /> </p> 
	    <p> <label for="user_prove">流水证明</label> <input id="user_prove" type="file" name="prove" /> </p> 
	    <p> <label for="user_phone">家庭电话</label> <input class="phone_group" id="user_phone" name="phone" /> </p> 
	    <p> <label for="user_mobile">个人电话</label> <input class="phone_group" id="user_mobile" name="mobile" /> </p> 
	    <p> <label for="user_image">个人头像</label> <input id="user_image" type="file" name="image" /> </p> 
	    <p> <label for="user_home">个人首页</label> <input id="user_home" type="url" name="home" /> </p> 
	    <p> <label for="user_password">输入密码</label> <input id="user_password" type="password" name="password" /> </p> 
	    <p> <label for="user_repassword">重复密码</label> <input id="user_repassword" type="password" name="repassword" /> </p> 
	    <p> <label for="send_to_me">邮件订阅</label> <input id="send_to_me" type="checkbox" name="sendMe" /> </p> 
	    <p> <label for="user_email">邮件地址</label> <input id="user_email" type="email" name="email" /> </p> 
	    <p> <label for="registration_agree">已阅读并同意注册协议</label> <input id="registration_agree" type="checkbox" name="regAgree" /> </p> 
	    <p> <input class="submit" type="submit" value="Submit" /> </p> 
	   </fieldset> 
	  </form> 
	  <div id="error_messages"> 
	   <div id="error_tips"></div> 
	   <ul></ul>
	  </div> 
	  <div id="error_container"></div> 
	  <script src="jquery-1.8.3.js"></script> 
	  <script src="jquery.validate.min.js"></script> 
	  <script src="additional-methods.min.js"></script> 
	  <script src="messages_zh.min.min.js"></script> 
	  <script>
		jQuery.validator.addMethod("math",
		function(value, element, params) {
			var sign = params[0];
			var result;
			switch (sign) {
	 
			case "*":
				result = params[1] * params[2];
				break;
			case "/":
				result = params[1] / params[2];
				break;
			case "-":
				result = params[1] - params[2];
				break;
			default:
				result = params[1] + params[2];
			}
			return this.optional(element) || value == result;
		},
		jQuery.validator.format("Please enter the correct value for {0} + {1} + {2}"));
		jQuery.validator.addClassRules("math_class", {
			required: true,
			math: ['*', 5, 6]
		});
		jQuery.validator.setDefaults({
			// 仅做校验，不提交表单
			debug: true,
			// 提交表单时做校验
			onsubmit: true,
			// 焦点自动定位到第一个无效元素
			focusInvalid: true,
			// 元素获取焦点时清除错误信息
			focusCleanup: true,
			//忽略 class="ignore" 的项不做校验
			ignore: ".ignore",
			// 忽略title属性的错误提示信息
			ignoreTitle: true,
			// 为错误信息提醒元素的 class 属性增加 invalid 
			errorClass: "invalid",
			// 为通过校验的元素的 class 属性增加 valid 
			validClass: "valid",
			// 使用 <em> 元素进行错误提醒
			errorElement: "em",
			// 使用 <li> 元素包装错误提醒元素
			wrapper: "li",
			// 将错误提醒元素统一添加到指定元素
			//errorLabelContainer: "#error_messages ul",
			// 自定义错误容器
			errorContainer: "#error_messages, #error_container",
			// 自定义错误提示如何展示
			showErrors: function(errorMap, errorList) {
				$("#error_tips").html("Your form contains " + this.numberOfInvalids() + " errors, see details below.");
				this.defaultShowErrors();
			},
			// 自定义错误提示位置
			errorPlacement: function(error, element) {
				error.insertAfter(element);
			},
			// 单个元素校验通过后处理
			success: function(label, element) {
				console.log(label);
				console.log(element);
				label.addClass("valid").text("Ok!")
			},
	 
			highlight: function(element, errorClass, validClass) {
				$(element).addClass(errorClass).removeClass(validClass);
				$(element.form).find("label[for=" + element.id + "]").addClass(errorClass);
			},
			unhighlight: function(element, errorClass, validClass) {
				$(element).removeClass(errorClass).addClass(validClass);
				$(element.form).find("label[for=" + element.id + "]").removeClass(errorClass);
			},
			//校验通过后的回调，可用来提交表单
			submitHandler: function(form, event) {
				console.log($(form).attr("id"));
				//$(form).ajaxSubmit();
				//form.submit();
			},
			//校验未通过后的回调
			invalidHandler: function(event, validator) {
				// 'this' refers to the form
				var errors = validator.numberOfInvalids();
				if (errors) {
					var message = errors == 1 ? 'You missed 1 field. It has been highlighted': 'You missed ' + errors + ' fields. They have been highlighted';
					console.log(message);
				}
			}
		});
		$("#example_form").validate({
			rules: {
				name: {
					required: true,
					minlength: 2,
					maxlength: 24
				},
				age: {
					required: true,
					rangelength: [1, 3],
					min: 10
				},
				birthday: {
					required: true,
					//date:true,
					dateISO: true
				},
				card: {
					required: {
						depends: function(element) {
							return $("#user_age").val() >= 18;
						}
					},
					creditcard: true
				},
				salary: {
					required: true,
					step: {
						param: 100,
						depends: function(element) {
							return $("#user_age").val() >= 28;
						}
					}
				},
				prove: {
					required: function(element) {
						return $("#user_age").val() >= 18;
					},
					extension: "xls|csv|doc"
				},
				phone: {
					require_from_group: [1, ".phone_group"],
					phoneUS: true
				},
				mobile: {
					require_from_group: [1, ".phone_group"]
				},
				image: {
					required: true,
					accept: "image/*"
					//accept:"audio/*,image/x-eps,application/pdf"
				},
				home: {
					required: true,
					url: true,
					// 校验之前对内容进行处理
					normalizer: function(element) {
						var url = $.trim($(element).val());
						// Check if it doesn't start with http:// or https:// or ftp://
						if (url && url.substr(0, 7) !== "http://" && url.substr(0, 8) !== "https://" && url.substr(0, 6) !== "ftp://") {
							// then prefix with http://
							url = "http://" + url;
						}
						// Return the new url
						return url;
					},
					// 失去焦点时进行校验
					onfocusout: function(element, event) {
						console.log(element);
					},
					onkeyup: function(element, event) {
						console.log(element);
					},
					onclick: function(element, event) {
						console.log(element);
					}
	 
				},
				password: {
					required: true,
					minlength: 6
				},
				repassword: {
					required: true,
					equalTo: "#user_password"
				},
				email: {
					required: "#send_to_me:checked",
					email: true,
					remote: {
						url: "http://localhost:8080/check/register",
						type: "post",
						dataType: "json",
						data: {
							email: $("#user_email").val()
						}
					}
				},
				regAgree: {
					required: true
				}
			},
			//自定义错误提示信息
			messages: {
				name: {
					required: "请输入用户名",
					minlength: jQuery.validator.format("用户名至少需填写{0}个字符"),
					maxlength: jQuery.validator.format("用户名最多填写{0}个字符")
				}
			},
		});
	</script>  
	 </body>
	</html>
