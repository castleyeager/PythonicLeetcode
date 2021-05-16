# 剑指 Offer 22. 链表中倒数第k个节点

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

 

示例：

给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.

## 题解

本题使用快慢指针，快指针fast领先慢指针slow k个节点，然后两指针一起以相同速度向前移动，此时快指针指向链表尾部，慢指针指向的就是链表倒数第 k 个节点。

```python
class Solution:
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
        # 初始化快慢指针
        slow = fast = head
        # 快指针领先慢指针 k 个节点
        for _ in range(k):
            fast = fast.next
        # 快慢指针同时向前移动
        while fast:
            fast = fast.next
            slow = slow.next
        # 此时慢指针指向的就是倒数第 k 个节点
        return slow
```

