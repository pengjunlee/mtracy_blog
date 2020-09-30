> 原文链接：<https://blog.csdn.net/mobile18611667978/article/details/88988884>

**第一种方式**是前端创建超链接，通过`<a>`标签的链接向后端服务发`get`请求，接收后端的文件流，非常简单：

	<a :href='"/user/downloadExcel"' >下载模板</a>

另一种情况是创建`<div>`标签，动态创建`<a>`标签：

	<div name="downloadfile" "downloadExcel()">下载</div>
	function downloadExcel() {
	    let a = document.createElement('a')
	    a.href ="/user/downloadExcel"
	    a.click();
	}

**第二种方式**通过创建`<iframe>`的方式：

	<el-button  size="mini" class="filter-item" type="primary" icon="el-icon-download" @click="handleExport(scope.row)">导出</el-button>
	//method方法：
	handleExport(row) {
	      var elemIF = document.createElement('iframe')
	      elemIF.src = 'user/downloadExcel?snapshotTime=' + formatDate(new Date(row.snapshotTime), 'yyyy-MM-dd hh:mm') +
	                    '&category=' + row.category 
	      elemIF.style.display = 'none'
	      document.body.appendChild(elemIF)
    }

**第三种方式**会对后端发的`post`请求，使用`blob`格式：

	<el-button  size="mini" class="filter-item" type="primary" icon="el-icon-download" @click="handleExport(scope.row)">导出</el-button>
	//method方法
	handleExport(row){
	const url="/user/downloadExcel"
	const options = {snapshotTime:formatDate(new Date(row.snapshotTime), 'yyyy-MM-dd hh:mm')}
	exportExcel(url,options)
	}
	/**
	 * 封装导出Excal文件请求
	 * @param url
	 * @param data
	 * @returns {Promise}
	 */
	//api.js
	export function exportExcel(url, options = {}) {
	  return new Promise((resolve, reject) => {
	    console.log(`${url} 请求数据，参数=>`, JSON.stringify(options))
	    axios.defaults.headers['content-type'] = 'application/json;charset=UTF-8'
	    axios({
	      method: 'post',
	      url: url, // 请求地址
	      data: options, // 参数
	      responseType: 'blob' // 表明返回服务器返回的数据类型
	    }).then(
	      response => {
	        resolve(response.data)
	        let blob = new Blob([response.data], {
	          type: 'application/vnd.ms-excel'
	        })
	        console.log(blob)
	        let fileName = Date.parse(new Date()) + '.xlsx'
	        if (window.navigator.msSaveOrOpenBlob) {
	          // console.log(2)
	          navigator.msSaveBlob(blob, fileName)
	        } else {
	          // console.log(3)
	          var link = document.createElement('a')
	          link.href = window.URL.createObjectURL(blob)
	          link.download = fileName
	          link.click()
	          //释放内存
	          window.URL.revokeObjectURL(link.href)
	        }
	      },
	      err => {
	        reject(err)
	      }
	    )
	  })
	}

如果后端提供的下载接口是`get`类型，可以直接使用**方法一**和**方法二**，简单又便捷；当然如果想使用**方法三**也是可以的，不过感觉有点舍近求远了。如果后端提供的下载接口是`post`类型，就必须要用方法三了。