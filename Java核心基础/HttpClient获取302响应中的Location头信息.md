# HttpClient获取302响应中的Location头信息

	public static String getLocationUrl(String url) {
		RequestConfig config = RequestConfig.custom().setConnectTimeout(50000).setConnectionRequestTimeout(10000).setSocketTimeout(50000)
                .setRedirectsEnabled(false).build();//不允许重定向   
		CloseableHttpClient httpClient = HttpClients.custom().setDefaultRequestConfig(config).build(); 
		String location = null;
		int responseCode = 0;
 
		HttpResponse response;
		try {
			response = httpClient.execute(new HttpGet(url));
			responseCode = response.getStatusLine().getStatusCode();
			if (responseCode == 302) {
				Header locationHeader = response.getFirstHeader("Location");
				location = locationHeader.getValue();
			}
		} catch (Exception e) {
			
			e.printStackTrace();
		}
		
		return location;
	}