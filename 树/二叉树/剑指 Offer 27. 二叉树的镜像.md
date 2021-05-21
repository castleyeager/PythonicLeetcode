# 剑指 Offer 27. 二叉树的镜像

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

```
	 4
   /   \
  2     7
 / \   / \
1   3 6   9
```

镜像输出：

```
	 4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**示例 1：**

输入：root = [4,2,7,1,3,6,9]

输出：[4,7,2,9,6,3,1]


限制：

0 <= 节点个数 <= 1000

## 题解

本题与[226. 翻转二叉树](https://github.com/CastleYeager/PythonicLeetcode/blob/main/%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91/226.%20%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.md)完全相同。

注意本题不能用中序遍历来做。因为如果先翻转左子树，然后交换左树右树位置再翻转右树，那么翻转的右树实际上是经过翻转后的原左树，那么右树又会被翻回原本的左树，这样就只能实现左右子树的交换，但没有实现翻转。

### 1、递归 - 前序遍历

```python
class Solution:
    def mirrorTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        # 递归 - 前序遍历
        root.left, root.right = root.right, root.left
        self.mirrorTree(root.left)
        self.mirrorTree(root.right)
        return root
```

### 2、递归 - 后序遍历

```python
class Solution:
    def mirrorTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        # 递归 - 后序遍历
        self.mirrorTree(root.left)
        self.mirrorTree(root.right)
        root.left, root.right = root.right, root.left
        return root
```

