# 使用net.sf.json遍历Json数组

	import org.junit.Test;
	import java.util.Iterator;
	import net.sf.json.JSONArray;
	import net.sf.json.JSONObject;
	 
	public class JsonArrayTest {
	 
		@SuppressWarnings("unchecked")
		@Test
		public void test1() {
			String arrStr = "[{key:'a',value:'1'},{key:'b',value:'2'},{key:'c',value:'3'}]";
			JSONArray jsonArray = JSONArray.fromObject(arrStr);
			for (int i = 0; i < jsonArray.size(); i++) {
				JSONObject jsonObj = jsonArray.getJSONObject(i);
				Iterator<String> iterator = jsonObj.keys();
				while (iterator.hasNext()) {
					String key = iterator.next();
					System.out.println(key + ":" + jsonObj.getString(key) + " ");
				}
			}
		}
	 
	}