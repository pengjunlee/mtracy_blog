> 转载自：https://blog.csdn.net/ai_zxc/article/details/50623560

	/*
	 *检索指定字符串在源字符串中的位置，如果存在返回第一次出现的位置，如果不存在返回0.
	 *@parameter tagstr string 被检索的目标字符串
	 *@parameter str    string 检索字符串
	 *@return int str在tagstr中出现的位置
	 */
	int INSTR(tagstr,str); 

用法：

	SELECT INSTR('foobarbar', 'bar'); 
	 =>  结果为 4
	SELECT INSTR('xbar', 'foobar');
	 =>  结果为 0

例：tbl_user 表的 roles 字段是一个由"，"分隔的角色列表字符串，要找出包含 " admin " 角色的用户。

	SELECT * FROM tbl_user WHERE INSTR(roles,'admin')>0