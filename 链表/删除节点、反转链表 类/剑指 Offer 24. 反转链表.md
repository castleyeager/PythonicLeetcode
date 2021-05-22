# 剑指 Offer 24. 反转链表

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

 

**示例:**

输入: 1->2->3->4->5->NULL

输出: 5->4->3->2->1->NULL

**限制：**

0 <= 节点个数 <= 5000

## 题解

本题和[206.反转链表](https://github.com/CastleYeager/PythonicLeetcode/blob/main/%E9%93%BE%E8%A1%A8/%E5%88%A0%E9%99%A4%E8%8A%82%E7%82%B9%E3%80%81%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8%20%E7%B1%BB/206.%20%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8.md)完全一样。

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head:
            return head
        prev = head
        while prev.next:
            cur = prev.next
            prev.next = cur.next
            cur.next = head
            head = cur
        return head
```

