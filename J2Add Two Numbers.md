2. Add Two Numbers

Medium

60851584Share

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

1. 递归

   代码的亮点在于，

   1. 使用递归，完美的实现了题目中需要倒置结果的需求。由于需要倒置结果（个位是linked list的头）使用以下代码，就可以在计算的同时，不断向下一位移动。

```java
ListNode ans = new ListNode(single);
ans.next = addTwoNumbers(l1.next, l2.next);
```



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        else{
            int sum = l1.val + l2.val;
            int single = sum % 10;
            int carry = sum / 10;
            ListNode ans = new ListNode(single);
            ans.next = addTwoNumbers(l1.next, l2.next);
            if(carry == 1){
                ans.next = addTwoNumbers(ans.next, new ListNode(1));
            }
            return ans;
        }
        
    }
}
```

syntax：

1. 声明新变量时，一定要声明变量类型
2. 递归调用函数时，直接调用函数名，注意函数的参数一定要与声明类型一致