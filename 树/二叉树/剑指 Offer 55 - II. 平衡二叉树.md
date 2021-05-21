# 剑指 Offer 55 - II. 平衡二叉树

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

 

**示例 1:**

给定二叉树 [3,9,20,null,null,15,7]

```
	3
   / \
  9  20
    /  \
   15   7
```

返回 true 。

**示例 2:**

给定二叉树 [1,2,2,3,3,null,null,4,4]

```
 	   1
	  / \
	 2   2
	/ \
   3   3
  / \
 4   4
```

返回 false 。

 

**限制：**

0 <= 树的结点个数 <= 10000

## 题解

### 1、递归 - DFS

```python
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        # 终止条件
        # 空树
        if not root:
            return True
        if abs(self.getdepth(root.left) - self.getdepth(root.right)) > 1:
            return False
        # 递归
        return self.isBalanced(root.left) and self.isBalanced(root.right)
    
    def getdepth(self, root):
        if not root:
            return 0
        queue = collections.deque([root])
        depth = 0
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

### 2、递归 - DFS - 优化

```python
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        return self.helper(root) != -1
    
    # 辅助函数 - 计算树的深度 - 由于用到左右子树的深度作为中间量，所以可以中间判断该树是否平衡
    # 从返回值来看 - 若树平衡，返回树的深度 - 若树不平衡，返回 -1
    def helper(self, root):
        # 终止条件
        if not root:
            return 0
        # 左右子树的深度
        left = self.helper(root.left)
        right = self.helper(root.right)
        # 若左右子树任一出现 -1深度，说明左右子树存在不平衡的树 - 或左右子树均平衡，但深度差大于1
        if left == -1 or right == -1 or abs(left - right) > 1:
            return -1
       	# 若左右子树均平衡且深度差不超过1，则说明当前树也是平衡的，返回树的深度
        return max(left, right) + 1
```

