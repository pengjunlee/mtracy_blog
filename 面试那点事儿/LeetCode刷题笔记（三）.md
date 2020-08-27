# 有效的括号
给定一个只包括`(`，`)`，`{`，`}`，`[`，`]` 的字符串，判断字符串是否有效。

有效字符串需满足：

- 左括号必须用相同类型的右括号闭合。
- 左括号必须以正确的顺序闭合。
- 注意空字符串可被认为是有效字符串。

**示例 1**:

	输入: "()"
	输出: true

**示例 2**:

	输入: "()[]{}"
	输出: true

**示例 3**:

	输入: "(]"
	输出: false

**示例 4**:

	输入: "([)]"
	输出: false

**示例 5**:

	输入: "{[]}"
	输出: true

**解法**：

	class Solution {
	    public boolean isValid(String s) {
	        if(s.length()%2 == 1 ){
	            return false;
	        }
	        Stack<Character> stack = new Stack<Character>();
	        Map<Character,Character> map = new HashMap<Character,Character>();
	        map.put(')','(');
	        map.put('}','{');
	        map.put(']','[');
	        for(char c:s.toCharArray()){
	            if(!stack.empty() && stack.peek() == map.get(c)){
	                stack.pop();
	            }else{
	                stack.push(c);
	            }
	        }
	        return stack.empty();
	    }
	}

# 合并两个有序链表
将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例**：

	输入：1->2->4, 1->3->4
	输出：1->1->2->3->4->4

**解法**：

	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
	        if( null == l1){
	            return l2;
	        }
	        if( null == l2){
	            return l1;
	        }
	 
	        if(l1.val <= l2.val){
	            l1.next = mergeTwoLists(l1.next,l2);
	            return l1;
	        }else{
	            l2.next = mergeTwoLists(l2.next,l1);
	            return l2;
	        }
	 
	 
	    }
	}

# 两两交换链表中的节点
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**示例**:

给定`1->2->3->4`, 你应该返回`2->1->4->3`.

	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public ListNode swapPairs(ListNode head) {
	        if(head == null){
	            return null;
	        }
	        if(head.next == null){
	            return head;
	        }
	        ListNode node = head.next;
	        head.next = swapPairs(node.next);
	        node.next = head;
	        return node;
	    }
	}
 