# 剑指 Offer 28. 对称的二叉树

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

​	1

   / \
  2   2
 / \ / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

​	1

   / \
  2   2
   \   \
   3    3

 

**示例 1：**

输入：root = [1,2,2,3,4,4,3]
输出：true

**示例 2：**

输入：root = [1,2,2,null,3,null,3]
输出：false

**限制：**

0 <= 节点个数 <= 1000

## 题解

本题与[101. 对称二叉树](https://github.com/CastleYeager/PythonicLeetcode/blob/main/%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91/101.%20%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.md)完全相同

### 1、递归 - DFS

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        return self.issame(root.left, root.right)

    def issame(self, p, q):
        # 均为空树
        if not p and not q:
            return True
        # 其中一个为空树
        if not p or not q:
            return False
        # 均为非空树
        return p.val == q.val and self.issame(p.left, q.right) and self.issame(p.right, q.left)
```

