# 字符串转换整数
请你来实现一个`atoi`函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

> <font color=red>注意</font>：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回`0`。

**说明**：

假设我们的环境只能存储`32`位大小的有符号整数，那么其数值范围为`[−2^31,  2^31 − 1]`。如果数值超过这个范围，请返回`INT_MAX (2^31 − 1)`或`INT_MIN (−2^31)`。

**示例 1**:

	输入: "42"
	输出: 42

**示例 2**:

	输入: "   -42"
	输出: -42
	解释: 第一个非空白字符为 '-', 它是一个负号。我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。

**示例 3**:

	输入: "4193 with words"
	输出: 4193
	解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。

**示例 4**:

	输入: "words and 987"
	输出: 0
	解释: 第一个非空字符是 'w', 但它不是数字或正、负号。因此无法执行有效的转换。

**示例 5**:

	输入: "-91283472332"
	输出: -2147483648
	解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 因此返回 INT_MIN (−2^31) 。

**解法一，使用正则**：

	//需要导入包
	import java.util.regex.*;
	class Solution {
	    public int myAtoi(String str) {
	        //清空字符串开头和末尾空格（这是trim方法功能，事实上我们只需清空开头空格）
	        str = str.trim();
	        // 构建正则表达式
	        Pattern p = Pattern.compile("^[\\+\\-]?\\d+");
	        Matcher m = p.matcher(str);
	        int value = 0;
	        //判断是否能匹配
	        if (m.find()){
	            String numStr = m.group(0);
	            try {
	                value = Integer.parseInt(numStr);
	            } catch (Exception e){
	                //由于有的字符串"42"没有正号，所以我们判断'-'
	                value = numStr.charAt(0) == '-' ? Integer.MIN_VALUE: Integer.MAX_VALUE;
	            }
	        }
	        return value;
	    }
	}

**解法二**：

	class Solution {
	    public int myAtoi(String str) {
	        //清空字符串开头和末尾空格（这是trim方法功能，事实上我们只需清空开头空格）
	        str = str.trim();
	        if(str.length()==0){
	            return 0;
	        }
	        StringBuilder sb = new StringBuilder();
	        char[] chars = str.toCharArray();
	        if(chars[0] >= '0' && chars[0] <= '9'){
	            for(char c:chars){
	                if(c >= '0' && c <= '9'){
	                    sb.append(c);
	                }else{
	                    break;
	                }
	            }
	        }else if((chars[0] == '+' || chars[0] == '-') && chars.length>1){
	            if(chars[1] >= '0' && chars[1] <= '9'){
	                sb.append(chars[0]);
	                sb.append(chars[1]);
	                for(int i = 2;i<chars.length;i++){
	                    if(chars[i] >= '0' && chars[i] <= '9'){
	                    sb.append(chars[i]);
	                    }else{
	                        break;
	                    }   
	                }
	            }else{
	                return 0;
	            }
	        }else{
	            return 0;
	        }
	       
	        int value = 0;
	        try {
	            value = Integer.parseInt(sb.toString());
	        } catch (Exception e){
	            //由于有的字符串"42"没有正号，所以我们判断'-'
	            value = sb.toString().charAt(0) == '-' ? Integer.MIN_VALUE: Integer.MAX_VALUE;
	        }
	        return value;
	    }
	}

# 回文数
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1**:

	输入: 121
	输出: true

**示例 2**:

	输入: -121
	输出: false
	解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

**示例 3**:

	输入: 10
	输出: false
	解释: 从右向左读, 为 01 。因此它不是一个回文数。

**解法一，将数字转为字符串**：

	class Solution {
	    public boolean isPalindrome(int x) {
	        if(x < 0){
	            return false;
	        }
	        String str1 =  x+"";
	        StringBuilder sb = new StringBuilder(str1);
	        String str2 = sb.reverse().toString();
	        return str1.equals(str2);
	    }
	}

**进阶:你能不将整数转为字符串来解决这个问题吗**？

	class Solution {
	    public boolean isPalindrome(int x) {
	        // 负数和能被十整除的不是回文数
	        if(x < 0 || ( x % 10 == 0 && x!= 0)){
	            return false;
	        }
	        int tmp = x;
	        int len = 0;
	        // 获取数字的长度
	        while(tmp>0){
	            tmp = tmp / 10;
	            len ++;
	        }
	        // 取一半长度，奇数长度忽略中位数
	        int count = len / 2;
	        int dev = 1;
	        for(int i = 0;i< count;i++){
	            dev *=10;
	        }
	        // 取又半部分数字
	        int right_half = x % dev;
	        // 左半部分数字反转
	        int left_half = len % 2 == 0 ? x/ dev: x/ (dev*10);
	        int left_reverse = 0;
	        while(left_half>0){
	            int m = left_half % 10;
	            left_half = left_half /10;
	            left_reverse = left_reverse * 10 + m;
	        }
	        return left_reverse == right_half;
	    }
	}

# 盛最多水的容器
给定`n`个非负整数`a1，a2，...，an`，每个数代表坐标中的一个点 `(i, ai)` 。在坐标内画`n`条垂直线，垂直线`i` 的两个端点分别为` (i, ai) `和 `(i, 0)`。找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

**说明**：你不能倾斜容器，且`n`的值至少为`2`。

图中垂直线代表输入数组 `[1,8,6,2,5,4,8,3,7]`。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为`49`。
<div align=center>

![LeetCode](./imgs/02.png "LeetCode示意图")
<div align=left>

**示例**:

	输入: [1,8,6,2,5,4,8,3,7]
	输出: 49

**解法一，暴力穷尽**：

	class Solution {
	    public int maxArea(int[] height) {
	        int maxArea = 0;
	        for(int i=0;i<height.length-1;i++ ){
	            for(int j = i +1;j<height.length;j++){
	                int area = (j-i)*Math.min(height[i],height[j]);
	                if(maxArea < area){
	                    maxArea = area;
	                }
	            }
	        }
	        return maxArea;
	    }
	}

方法二，双向指针：
<div align=center>

![LeetCode](./imgs/03.png "LeetCode示意图")
<div align=left>

	class Solution {
	    public int maxArea(int[] height) {
	        int maxArea = 0;
	        int len = height.length;
	        for(int i=0;i<len-1;i++ ){
	            for(int j = len -1;j>i;j--){
	                if(height[j]>=height[i]){
	                    maxArea = Math.max(maxArea,height[i]*(j-i));
	                    break;
	                }else{
	                    maxArea = Math.max(maxArea,height[j]*(j-i));
	                }
	            }
	        }
	        return maxArea;
	    }
	}

# 最长公共前缀
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

**示例 1**:

	输入: ["flower","flow","flight"]
	输出: "fl"

**示例 2**:

	输入: ["dog","racecar","car"]
	输出: ""
	解释: 输入不存在公共前缀。

**说明**:所有输入只包含小写字母`a-z`。

**解法**：

	class Solution {
	    public String longestCommonPrefix(String[] strs) {
	        String prefix = null;
	        for(String str : strs){
	            if(prefix == null){
	                prefix = str;
	            }else if(prefix.length()>str.length()){
	                prefix = str;
	            }
	        }
	        if(prefix == null || prefix.equals("")){
	            return "";
	        }
	        int index = prefix.length();
	        for(String str:strs){
	            if(!str.equals(prefix)){
	                for(int i = 0 ;i<index;i++){
	                    if(str.charAt(i) != prefix.charAt(i)){
	                        index = i;
	                        break;
	                    }
	                }
	            }
	        }
	        return prefix.substring(0,index);
	    }
	}

# 删除链表的倒数第N个节点
给定一个链表，删除链表的倒数第`n`个节点，并且返回链表的头结点。

**示例**：

给定一个链表: `1->2->3->4->5`, 和 `n = 2`.

当删除了倒数第二个节点后，链表变为`1->2->3->5`.

**说明**：给定的`n`保证是有效的。

**进阶**：你能尝试使用一趟扫描实现吗？
<div align=center>

![LeetCode](./imgs/04.gif "LeetCode示意图")
<div align=left>


	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public ListNode removeNthFromEnd(ListNode head, int n) {
	        if(head.next == null){
	            return null;
	        }
	        ListNode node = head;
	        ListNode beforeNode = null;
	        while(node!=null){
	            if(n == 0){
	                beforeNode = beforeNode == null? head: beforeNode.next;
	            }else{
	                n-- ;
	            }
	            node = node.next;
	        } 
	        if(null==beforeNode){
	            head = head.next;
	        }else{
	            beforeNode.next = beforeNode.next.next;
	        }
	        return head;
	    }
	}