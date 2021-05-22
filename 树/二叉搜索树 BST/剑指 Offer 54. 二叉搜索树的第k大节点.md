# 剑指 Offer 54. 二叉搜索树的第k大节点

给定一棵二叉搜索树，请找出其中第k大的节点。

 

**示例 1:**

输入: root = [3,1,4,null,2], k = 1

```
   3
  / \
 1   4
  \
   2
```

输出: 4

**示例 2:**

输入: root = [5,3,6,2,4,null,null,1], k = 3
     

```
  	   5
      / \
     3   6
    / \
   2   4
  /
 1
```

输出: 4


限制：

- 1 ≤ k ≤ 二叉搜索树元素个数

## 题解

### 1、迭代 - 中序遍历 - 双端队列deque

主要思路：搜索采用中规中矩的中序遍历，维护一个最大长度为 k 的双端队列，该队列时刻维护此时树中值最大的 k 个元素，等遍历完整棵树后，队首元素即为整棵树中第 k 大的元素。

注意：本题虽没有明确规定，但从这段代码能通过全部用例这一点可以看出，本题的BST隐含着一个条件：节点值不相等，或者说左子树的值都是严格小于根节点值，右子树的值都是严格大于根节点值的。如果没有这个隐含条件，那么我们就不能只用双端队列做，而应该加上可以查重的哈希表来做。

```python
class Solution:
    def kthLargest(self, root: TreeNode, k: int) -> int:
        # 迭代 - DFS
        # 初始化双端队列
        nums = collections.deque(maxlen = k)
        # 迭代 - DFS - 中序遍历
        stack = []
        node = root
        while stack or node:
            while node:
                stack.append(node)
                node = node.left
            node = stack.pop()
            # 关键逻辑 - 当前节点值入队
            nums.append(node.val)
            node = node.right
        # 当前队列的队首元素即为 第k大的元素
        return nums[0]
```

### 2、迭代 - 中序遍历 - 哈希表查重

这里我就假设本题没有隐含节点值不相等的条件，或者说以后本题的测试用例新增了一些含有相等节点值的树作为用例，那么我们就不能只用双端队列了，因为重复元素会占据多个“排名”，我们需要考虑查重的问题，因此试着写了一段以哈希表和双端队列一起求解的代码，主要逻辑和 方法1 完全一样，只是将双端队列的操作新增了哈希表。

```python
class Solution:
    def kthLargest(self, root: TreeNode, k: int) -> int:
        # 迭代 - DFS
        nums = collections.deque(maxlen = k)
        # 新增的哈希表 - 用于查重
        s = set()
        stack = []
        node = root
        while stack or node:
            while node:
                stack.append(node)
                node = node.left
            node = stack.pop()
            # 新增逻辑 - 入队前需检查是否重复 - 重复则不做处理 - 不重复则加入排名
            if node.val not in s:
                nums.append(node.val)
                s.add(node.val)
            node = node.right
        return nums[0]
```

### 3、迭代 - 中序遍历 - 前驱节点查重

在 方法2 的基础上，维护一个哈希表的代价恐怕还是有点大，所以我考虑了BST很多题都会用到的方法——维护一个前驱节点值。

为什么维护一个前驱节点值就可以代替哈希表呢？因为BST的中序遍历是一个非递减序列，重复元素只可能出现在连续的位置，若我们能判断当前节点和前驱节点的大小关系，就可以知道当前节点是否重复，换句话说，当前节点是否重复，只需要和前驱节点做判断即可，完全不需要维护哈希表，哈希表包含的信息太多，而且是我们用不上的

```python
class Solution:
    def kthLargest(self, root: TreeNode, k: int) -> int:
        # 迭代 - DFS
        nums = collections.deque(maxlen = k)
        # 初始化前驱节点值
        prev_val = None
        stack = []
        node = root
        while stack or node:
            while node:
                stack.append(node)
                node = node.left
            node = stack.pop()
            # 关键逻辑 - 当前节点需要与前驱节点做判断 - 用于查重
            # 若当前节点与前驱节点不同 - 当前节点值入队，更新前驱节点信息
            if node.val != prev_val:
                nums.append(node.val)
                prev_val = node.val
            node = node.right
        return nums[0]
```

### 4、迭代 - 中序遍历的倒序 

BST的性质中，最常用的就是——BST中序遍历为一个非递减的序列

而另外还有一个性质，那就是BST中序遍历的倒序是一个非递增的序列

这里我用的 “中序遍历的倒序” 与 “后序遍历”是两个概念，具体是这么区分的：

中序遍历的倒序：按照中序遍历来搜索，但是先深搜右子树，再输出根节点，再深搜左子树。

右 - 根 - 左

后序遍历：先深搜左子树，再深搜右子树，最后输出根节点。 

左 - 右 - 根

这段代码结合了 “中序遍历的倒序” 和 前驱节点查重 两个点。

```python
class Solution:
    def kthLargest(self, root: TreeNode, k: int) -> int:
        # 初始化前驱节点值
        prev_val = None
        # 迭代 - DFS - 模板 - 右 根 左
        stack = []
        node = root
        while stack or node:
            # 右
            while node:
                stack.append(node)
                node = node.right
           	# 根
            node = stack.pop()
            # 关键逻辑 - 若 k 为 1，直接返回当前节点值
            if k == 1:
                return node.val
           	# 若 k 不为 1，则还需要继续搜索 - 若当前节点值与前驱节点值相等，则不占排名，k不递减
            # 若当前节点值与前驱节点值不相等，说明当前节点需要占一个排名，k需要递减1，同时更新前驱节点
            if node.val != prev_val:
                k -= 1
                prev_val = node.val
            node = node.left
```

