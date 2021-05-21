# 剑指 Offer 55 - I. 二叉树的深度

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

例如：

给定二叉树 [3,9,20,null,null,15,7]，

​	3

   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。

 

提示：

节点总数 <= 10000

## 题解

本题与[104. 二叉树的最大深度](https://github.com/CastleYeager/PythonicLeetcode/blob/main/%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91/104.%20%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%A4%A7%E6%B7%B1%E5%BA%A6.md)完全相同。

### 1、迭代 - BFS

```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        # BFS
        depth = 0
        queue = collections.deque([root])
        while queue:
            depth += 1
            l = len(queue)
            for _ in range(l):
                node = queue.popleft()
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        return depth
```

### 2、递归 - DFS	

```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        # 终止条件
        if not root:
            return 0
        # 递归
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```

