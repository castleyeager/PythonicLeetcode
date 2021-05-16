# 剑指 Offer 06. 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

**示例 1：**

输入：head = [1,3,2]

输出：[2,3,1]

## 题解

本题需要反向输出链表节点值，有两种思路：

1、遍历链表，正向保存节点值，反转输出接口

2、反转链表，然后正向遍历输出链表值

由于本题是链表题，所以我选择方法2，不使用链表以外的序列，因此本题本质上是反转链表。

详细可以参考[206.反转链表](https://github.com/CastleYeager/PythonicLeetcode/blob/main/%E9%93%BE%E8%A1%A8/%E5%88%A0%E9%99%A4%E8%8A%82%E7%82%B9%E3%80%81%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8%20%E7%B1%BB/206.%20%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8.md)

```python
class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        # 特判
        if not head:
            return []
        # 反转链表
        prev = head
        while prev.next:
            cur = prev.next
            prev.next = cur.next
            cur.next = head
            head = cur 
        # 遍历反转后的链表
        res = []
        cur = head
        while cur:
            res.append(cur.val)
            cur = cur.next
        return res
```

