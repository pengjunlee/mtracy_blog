# 两数之和
给定一个整数数组`nums`和一个目标值`target`，请你在该数组中找出和为目标值的那**两个**整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例**:

	# 给定 
	nums = [2, 7, 11, 15], target = 9
	 
	# 因为 
	nums[0] + nums[1] = 2 + 7 = 9
	 
	#所以返回 
	[0, 1]

**解法**：

	class Solution {
	    public int[] twoSum(int[] nums, int target) {
	        for(int i=0;i<nums.length-1;i++){
	            for(int j = i+1;j<nums.length;j++){
	                if(nums[j] == target - nums[i]){
	                    return new int[]{i,j};
	                }
	            }
	        }
	        throw new Error("答案不存在！");
	    }
	}

# 无重复字符的最长子串
给定一个字符串，请你找出其中不含有重复字符的`最长子串`的长度。

**示例 1**:

	输入: "abcabcbb"
	输出: 3 
	解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

**示例 2**:

	输入: "bbbbb"
	输出: 1
	解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

**示例 3**:

	输入: "pwwkew"
	输出: 3
	解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。

请注意，你的答案必须是`子串`的长度，"pwke"是一个子序列，不是子串。

**解法**：

	class Solution {
	    public int lengthOfLongestSubstring(String s) {
	        // 先获取不重复字符的个数
	        Set<Character> charSet = new HashSet<>(); 
	        for(int i = 0;i<s.length();i++){
	            charSet.add(s.charAt(i));
	        }
	        int maxLength = charSet.size();
	        int strLength = s.length();
	        int i = 0 ,j = 0,ret = 0;
	        Set<Character> set = new HashSet<>(); 
	        while( i<strLength && j<strLength && ret < maxLength){
	            if(!set.contains(s.charAt(j))){
	                set.add(s.charAt(j++));
	                ret = Math.max(ret, j - i);
	 
	            }else{
	                set.remove(s.charAt(i++));
	            }
	        }
	        return ret;
	    }
	}

# 最长回文字符串
给定一个字符串`s`，找到`s`中最长的回文子串。你可以假设`s`的最大长度为`1000`。

**示例 1**：

	输入: "babad"
	输出: "bab"
	注意: "aba" 也是一个有效答案。

**示例 2**：

	输入: "cbbd"
	输出: "bb"

**解法**：

	class Solution {
	    public String longestPalindrome(String s) {
	        int len = s.length();
	        if(s.length()<=1){
	            return s;
	        }
	        String ret = "";
	        for (int i = 0;i<len -1;i++){
	            int l = i;
	            int r = i;
	            boolean b = true;
	            while( true){
	                if(r<len-1 && b && s.charAt(r+1) == s.charAt(i)){
	                    r++;
	                    i++;
	                }else if(l>0 && r<len-1 && s.charAt(l-1) == s.charAt(r+1)){
	                    l--;         
	                    r++;
	                    b = false;
	                }else{
	                    break;
	                }
	            }
	            String tmp = s.substring(l,r+1);
	            if(ret.length() < tmp.length()){
	                ret = tmp;
	            }
	        }
	        return ret;
	    }
	}

# Z 字形变换
将一个给定字符串根据给定的行数，以从上往下、从左到右进行`Z`字形排列。

比如输入字符串为`LEETCODEISHIRING`行数为 3 时，排列如下：

	L   C   I   R
	E T O E S I I G
	E   D   H   N

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`LCIRETOESIIGEDHN`。

请你实现这个将字符串进行指定行数变换的函数：

	string convert(string s, int numRows);

**示例 1**:

	输入: s = "LEETCODEISHIRING", numRows = 3
	输出: "LCIRETOESIIGEDHN"

**示例 2**:

	输入: s = "LEETCODEISHIRING", numRows = 4
	输出: "LDREOEIIECIHNTSG"

**解释**:

	L     D     R
	E   O E   I I
	E C   I H   N
	T     S     G

**解法**：

	class Solution {
	    public String convert(String s, int numRows) {
	        if (numRows == 1) return s;
	 
	        List<StringBuilder> rows = new ArrayList<>();
	        for (int i = 0; i < Math.min(numRows, s.length()); i++)
	            rows.add(new StringBuilder());
	 
	        int curRow = 0;
	        boolean goingDown = false;
	 
	        for (char c : s.toCharArray()) {
	            rows.get(curRow).append(c);
	            if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
	            curRow += goingDown ? 1 : -1;
	        }
	 
	        StringBuilder ret = new StringBuilder();
	        for (StringBuilder row : rows) ret.append(row);
	        return ret.toString();
	    }
	}

# 整数反转
给出一个`32`位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例 1**:

	输入: 123
	输出: 321

**示例 2**:

	输入: -123
	输出: -321

**示例 3**:

	输入: 120
	输出: 21

**注意**:

假设我们的环境只能存储得下`32`位的有符号整数，则其数值范围为`[−231,  231 − 1]`。请根据这个假设，如果反转后整数溢出那么就返回`0`。

**解法**：

	class Solution {
	    public int reverse(int x) {
	        int rev = 0;
	        while(x != 0){
	            int pop = x % 10;
	            x = x / 10;
	            if(rev > Integer.MAX_VALUE / 10 || (rev == Integer.MAX_VALUE / 10 && pop > Integer.MAX_VALUE % 10)){
	                rev = 0;
	                break;
	            }else if(rev < Integer.MIN_VALUE / 10 || (rev == Integer.MIN_VALUE / 10 && x < Integer.MIN_VALUE % 10)){
	                rev = 0;
	                break;
	            }
	            rev = rev * 10 + pop;
	        }
	        return rev;
	    }
	}
 

