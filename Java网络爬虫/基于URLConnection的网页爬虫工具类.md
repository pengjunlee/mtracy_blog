在这个数据为王的时代，爬虫应用地越来越广泛，对于一个萌新程序员来说如果你要做爬虫，那么Python是你的不二之选。但是对于那些老腊肉的Java程序员（亦或者你是程序媛）想使用Java做爬虫也不是不行，只是没有Python那么方便。身为一块Java老腊肉的我在此记录一下自己在使用Java做网络爬虫使用的工具类。

在`pom.xml`文件中引入`commons-lang3`依赖：

		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>3.6</version>
		</dependency>

`SpiderHttpUtils`工具类完整代码如下： 

	import java.io.BufferedInputStream;
	import java.io.BufferedReader;
	import java.io.ByteArrayOutputStream;
	import java.io.InputStreamReader;
	import java.io.UnsupportedEncodingException;
	import java.net.HttpURLConnection;
	import java.net.URL;
	import java.net.URLConnection;
	import java.net.URLEncoder;
	import java.security.cert.CertificateException;
	import java.security.cert.X509Certificate;
	import java.util.Map;
	 
	import javax.net.ssl.HttpsURLConnection;
	import javax.net.ssl.SSLContext;
	import javax.net.ssl.SSLSocketFactory;
	import javax.net.ssl.TrustManager;
	import javax.net.ssl.X509TrustManager;
	 
	import org.apache.commons.lang3.StringUtils;
	 
	public class SpiderHttpUtils {
	 
		public static String sendGet(boolean isHttps, String requestUrl, Map<String, String> params,
				Map<String, String> headers, String charSet) {
			if (StringUtils.isBlank(requestUrl)) {
				return "";
			}
			if (StringUtils.isBlank(charSet)) {
				charSet = "UTF-8";
			}
			URL url = null;
			URLConnection conn = null;
			BufferedReader br = null;
	 
			try {
				// 创建连接
				url = new URL(requestUrl + "?" + requestParamsBuild(params));
				if (isHttps) {
					conn = getHttpsUrlConnection(url);
				} else {
					conn = (HttpURLConnection) url.openConnection();
				}
	 
				// 设置请求头通用属性
	 
				// 指定客户端能够接收的内容类型
				conn.setRequestProperty("Accept", "*/*");
	 
				// 设置连接的状态为长连接
				conn.setRequestProperty("Connection", "keep-alive");
	 
				// 设置发送请求的客户机系统信息
				conn.setRequestProperty("User-Agent",
						"Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36");
	 
				// 设置请求头自定义属性
				if (null != headers && headers.size() > 0) {
	 
					for (Map.Entry<String, String> entry : headers.entrySet()) {
						conn.setRequestProperty(entry.getKey(), entry.getValue());
					}
				}
	 
				// 设置其他属性
				// conn.setUseCaches(false);//不使用缓存
				// conn.setReadTimeout(10000);// 设置读取超时时间
				// conn.setConnectTimeout(10000);// 设置连接超时时间
	 
				// 建立实际连接
				conn.connect();
	 
				// 读取请求结果
				br = new BufferedReader(new InputStreamReader(conn.getInputStream(), charSet));
				String line = null;
				StringBuilder sb = new StringBuilder();
				while ((line = br.readLine()) != null) {
					sb.append(line);
				}
				return sb.toString();
			} catch (Exception exception) {
				return "";
			} finally {
				try {
					if (br != null) {
						br.close();
					}
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
	 
		}
	 
		public static String requestParamsBuild(Map<String, String> map) {
			String result = "";
			if (null != map && map.size() > 0) {
				StringBuffer sb = new StringBuffer();
				for (Map.Entry<String, String> entry : map.entrySet()) {
					try {
						String value = URLEncoder.encode(entry.getValue(), "UTF-8");
						sb.append(entry.getKey() + "=" + value + "&");
					} catch (UnsupportedEncodingException e) {
						e.printStackTrace();
					}
				}
	 
				result = sb.substring(0, sb.length() - 1);
			}
			return result;
		}
	 
		private static HttpsURLConnection getHttpsUrlConnection(URL url) throws Exception {
			HttpsURLConnection httpsConn = (HttpsURLConnection) url.openConnection();
			// 创建SSLContext对象，并使用我们指定的信任管理器初始化
			TrustManager[] tm = { new X509TrustManager() {
				public void checkClientTrusted(X509Certificate[] chain, String authType) throws CertificateException {
					// 检查客户端证书
				}
	 
				public void checkServerTrusted(X509Certificate[] chain, String authType) throws CertificateException {
					// 检查服务器端证书
				}
	 
				public X509Certificate[] getAcceptedIssuers() {
					// 返回受信任的X509证书数组
					return null;
				}
			} };
			SSLContext sslContext = SSLContext.getInstance("SSL", "SunJSSE");
			sslContext.init(null, tm, new java.security.SecureRandom());
			// 从上述SSLContext对象中得到SSLSocketFactory对象
			SSLSocketFactory ssf = sslContext.getSocketFactory();
			httpsConn.setSSLSocketFactory(ssf);
			return httpsConn;
	 
		}
	 
		public static byte[] getFileAsByte(boolean isHttps, String requestUrl) {
			if (StringUtils.isBlank(requestUrl)) {
				return new byte[0];
			}
			URL url = null;
			URLConnection conn = null;
			BufferedInputStream bi = null;
	 
			try {
				// 创建连接
				url = new URL(requestUrl);
				if (isHttps) {
					conn = getHttpsUrlConnection(url);
				} else {
					conn = (HttpURLConnection) url.openConnection();
				}
	 
				// 设置请求头通用属性
	 
				// 指定客户端能够接收的内容类型
				conn.setRequestProperty("accept", "*/*");
	 
				// 设置连接的状态为长连接
				conn.setRequestProperty("Connection", "keep-alive");
	 
				// 设置发送请求的客户机系统信息
				conn.setRequestProperty("User-Agent", "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1;SV1)");
				// 设置其他属性
				conn.setConnectTimeout(3000);// 设置连接超时时间
	 
				conn.setDoOutput(true);
				conn.setDoInput(true);
	 
				// 建立实际连接
				conn.connect();
	 
				// 读取请求结果
				bi = new BufferedInputStream(conn.getInputStream());
				ByteArrayOutputStream outStream = new ByteArrayOutputStream();
				byte[] buffer = new byte[2048];
				int len = 0;
				while ((len = bi.read(buffer)) != -1) {
					outStream.write(buffer, 0, len);
				}
				bi.close();
				byte[] data = outStream.toByteArray();
				return data;
			} catch (Exception exception) {
				return new byte[0];
			} finally {
				try {
					if (bi != null) {
						bi.close();
					}
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
	 
		}
	 
	}
