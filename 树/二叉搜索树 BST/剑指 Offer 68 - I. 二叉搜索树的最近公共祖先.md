# 剑指 Offer 68 - I. 二叉搜索树的最近公共祖先

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)

**示例 1:**

输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8

输出: 6 

解释: 节点 2 和节点 8 的最近公共祖先是 6。

**示例 2:**

输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4

输出: 2

解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。

**说明:**

- 所有节点的值都是唯一的。
- p、q 为不同节点且均存在于给定的二叉搜索树中。

## 题解

本题与[235. 二叉搜索树的最近公共祖先](https://github.com/CastleYeager/PythonicLeetcode/blob/main/%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%20BST/235.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.md)完全相同。

### 1、递归 - DFS - 二分

提一嘴，若目标节点分别在左右子树中，那么目标节点的LCA一定是root，因为两棵子树的最近祖先一定是将两棵子树维系起来的 root 节点。

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 空树
        if not root:
            return None
        # 非空树
        # 若当前节点就是目标节点之一 - 当前节点即为LCA
        if root == p or root == q:
            return root
        # 若目标节点值都大于当前节点值，则目标节点只可能出现在右子树中 - 递归搜索右子树
        if root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p ,q)
        # 若目标节点值都小于当前节点值，则目标节点只可能出现在左子树中 - 递归搜索左子树
        elif root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        # 剩余情况为，目标元素值分别位于当前节点值的两侧，则当前节点即为LCA
        return root
```

