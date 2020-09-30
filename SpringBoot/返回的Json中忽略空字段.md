# 第一种：

	@JsonInclude(JsonInclude.Include.NON_NULL)

# 第二种：

	spring:
	  jackson:
	    default-property-inclusion: non_null
 