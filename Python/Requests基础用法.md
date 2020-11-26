> 原文地址：<https://blog.csdn.net/LKJLKJKL/article/details/95049190>

**FilesPipeline**的工作流如下：

- 在spider中爬取要下载的文件链接，将其放置于item中的file_urls（注意这只是一个代名词就像数学中的x，他的值在配置项里面，可以自定义的）。
- spider将其返回并传送至pipeline。
- 当FilesPipeline处理时，它会检测是否有file_urls字段，如果有的话，会将url传送给scarpy调度器和下载器。
- 下载完成之后，会将结果写入item的另一字段files，files包含了文件现在的本地路径（相对于配置FILE_STORE的路径）、文件校验和checksum、文件的url

两个管道都实现了这些功能：

- 避免重新下载最近下载的媒体
- 指定存储介质的位置（文件系统目录）
- 图像管道具有一些用于处理图像的额外功能：1. 将下载的图片转换为JPG格式和RGB模式，并生成图像缩略图；2. 检查图像宽度/高度以确保它们符合最小约束（需要在settings中配置）；

在`settings`中，对图像管道进行配置：

	ITEM_PIPELINES  =  { 'scrapy.pipelines.images.ImagesPipeline' ： 1 }
	IMAGES_STORE =  '/path/to/valid/dir'

对文件管道进行配置：

	ITEM_PIPELINES  =  { 'scrapy.pipelines.files.FilesPipeline' ： 1 }
	FILES_STORE=  '/path/to/valid/dir'

文件的和图片将使用url的`sha-1`散列值进行命名，例如：

	http://www.example.com/image.jpg
	# 对应
	3afec3b4765f8f0a07b78f98c07b83f013567a0a
	# 最后的文件名是
	3afec3b4765f8f0a07b78f98c07b83f013567a0a.jpg

下载的图片将存储在以下路径：

	<IMAGES_STORE>/full/3afec3b4765f8f0a07b78f98c07b83f013567a0a.jpg

例子：

	import scrapy
	class MyItem(scrapy.Item):
	    # ... other item fields ...
	    image_urls = scrapy.Field()
	    images = scrapy.Field()
	    file_urls = scrapy.Field()
	    files = scrapy.Field()

自定义`file_urls`的存储字段和结果信息字段名称，在配置文件做如下设置：

	# For the Files Pipeline
	 
	FILES_URLS_FIELD = 'field_name_for_your_files_urls'
	FILES_RESULT_FIELD = 'field_name_for_your_processed_files'
	
	# For the Images Pipeline    
	   
	IMAGES_URLS_FIELD = 'field_name_for_your_images_urls'
	IMAGES_RESULT_FIELD = 'field_name_for_your_processed_images'

还有一些附加功能。

文件寿命配置：

    # 120 days of delay for files expiration
    FILES_EXPIRES = 120   （配置文件的寿命，这在连续生产环境将非常有用）
    # 图像到期延迟30天
    IMAGES_EXPIRES  =  30
    # 如果你使用的是自定义的：文件处理流水线（MYPIPELINE），那么使用如下配置：
    (MYPIPELINE)_FILES_EXPIRES = 180

缩略图配置项：

	IMAGES_THUMBS = {
	    'small': (50, 50),
	    'big': (270, 270),
	}

Example ：

	<IMAGES_STORE>/full/63bbfea82b8880ed33cdb762aa11fab722a90a24.jpg
	<IMAGES_STORE>/thumbs/small/63bbfea82b8880ed33cdb762aa11fab722a90a24.jpg
	<IMAGES_STORE>/thumbs/big/63bbfea82b8880ed33cdb762aa11fab722a90a24.jpg

过滤尺寸过大过小的图片：

	IMAGES_MIN_HEIGHT = 110
	IMAGES_MIN_WIDTH = 110

自定义图像处理流水线（pipeline）的完整例子：

	# Custom Images pipeline example：
	# https://doc.scrapy.org/en/1.3/topics/media-pipeline.html#custom-images-pipeline-example
	 
	import scrapy
	from scrapy.pipelines.images import ImagesPipeline
	from scrapy.exceptions import DropItem
	 
	class MyImagesPipeline(ImagesPipeline):
	 
	    def get_media_requests(self, item, info):
	        for image_url in item['image_urls']:
	            yield scrapy.Request(image_url)
	 
	    def item_completed(self, results, item, info):
	        image_paths = [x['path'] for ok, x in results if ok]
	        if not image_paths:
	            raise DropItem("Item contains no images")
	        item['image_paths'] = image_paths
	        return item

# 参考文章

<https://blog.csdn.net/qq_43537354/article/details/88360636>

<https://doc.scrapy.org/en/1.3/topics/media-pipeline.html>