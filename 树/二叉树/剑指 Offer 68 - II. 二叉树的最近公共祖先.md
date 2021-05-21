# 剑指 Offer 68 - II. 二叉树的最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)

**示例 1:**

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1

输出: 3

解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
**示例 2:**

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4

输出: 5

解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。

**说明:**

- 所有节点的值都是唯一的。
- p、q 为不同节点且均存在于给定的二叉树中。

## 题解

### 1、递归 - DFS

本题与 236. 二叉树的最近公共祖先 完全相同

在一棵树root中找 p 和 q 的最大公共祖先，有这几种情况：

1、root就是 p q 之一，则root就是LCA

2、p q 在root的左子树或右子树中（同侧子树），则问题转换到 在 root.left 或 root.right 中寻找 p q 的LCA

3、p q 在root的异侧子树中，则root就是LCA

```python
class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        # 当前节点应该做什么
        if not root:
            return None
        # 若根节点就是目标之一 - root就是LCA
        if p == root or q == root:
            return root
        # 递归得到左右子树中的搜索结果
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        # 若左树未找到 - 说明pq都在右树
        if not left:
            return right
        # 若右树未找到 - 说明pq都在左树
        if not right:
            return left
        # 若左右树都有找到 - 说明root即为LCA
        return root
```

