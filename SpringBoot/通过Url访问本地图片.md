> 转载自：<https://blog.csdn.net/chao821/article/details/85565231>

在springboot项目中，如何将本地的图片映射成为资源，并使其可以通过HTTP请求url访问？

只需要增加一个配置类：

	@Configuration
	public class WebMvcConfigurer extends WebMvcConfigurerAdapter {
	 
	    @Override
	    public void addResourceHandlers(ResourceHandlerRegistry registry) {
	        //和页面有关的静态目录都放在项目的static目录下
	        registry.addResourceHandler("/static/**").addResourceLocations("classpath:/static/");
	        //上传的图片在D盘下的OTA目录下，访问路径如：http://localhost:8081/OTA/d3cf0281-bb7f-40e0-ab77-406db95ccf2c.jpg
	        //其中OTA表示访问的前缀。"file:D:/OTA/"是文件真实的存储路径
	        registry.addResourceHandler("/OTA/**").addResourceLocations("file:D:/OTA/");
	    }
	}

运行该工程：可以发现资源文件夹static也被放入了部署的target文件夹中。

<div align=center>![项目示意图](./imgs/03.png "项目示意图")
<div align=left>

另外，通过以下网址均可访问相关静态资源：

	http://localhost:8080/static/%E6%8D%95%E8%8E%B7.PNG
	http://localhost:8080/OTA/%E6%8D%95%E8%8E%B7.PNG