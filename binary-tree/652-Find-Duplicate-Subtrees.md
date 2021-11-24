#### Description

Given the `root` of a binary tree, return all **duplicate subtrees**.

For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.

Two trees are **duplicate** if they have the **same structure** with the **same node values**.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/16/e1.jpg)

```markdown
Input: root = [1,2,3,4,null,2,4,null,null,4]
Output: [[2,4],[4]]
```



#### Solution 1: Serialization

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findDuplicateSubtrees(self, root: Optional[TreeNode]) -> List[Optional[TreeNode]]:
        memo = {}
        res = []
        
        def postorder(root):
            if not root:
                return "#"
            
            left = postorder(root.left)
            right = postorder(root.right)
            
            subtree = left + "," + right + "," + str(root.val)
            
            if subtree not in memo:
                memo[subtree] = 0
            
            if memo[subtree] == 1:
                res.append(root)
            
            memo[subtree] += 1
            return subtree
        
        postorder(root)
        
        return res
```

1. 利用二叉树序列化，通过postorder travesal 描述subtree 的结构， `#` 表示空指针，`,` 分隔每个二叉树的节点值；
2. 利用hashmap 记录subtree的freq, 如果subtree freq == 1, 将当前root节点存入res中

Time complexity: O(n^2)
Space complexity: O(n^2)

For the best case, binary tree is a perfect tree, time complexity T(n) = O(n) + 2* T(n/2) = O(nlogn)

For the worst case, binary tree is a skewed tree such as only left subtree has nodes, T(n) = O(n) + T(n-1) = O(n^2)

n is the number of tree nodes. O(n) means for root node

#### Solution 2: assign int id for each unique subtree

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findDuplicateSubtrees(self, root: Optional[TreeNode]) -> List[Optional[TreeNode]]:
        visited = set()
        res = []
        subtrees = {}
        idx = 1
        
        def postorder(root):
            nonlocal idx 
            if not root:
                return 0
            left_idx = postorder(root.left)
            right_idx = postorder(root.right)
            
            key = (left_idx, right_idx, root.val)
            
            if key in subtrees:
                if key not in visited:
                    visited.add(key)
                    res.append(root)
            else:
                subtrees[key] = idx
                idx += 1
            
            return subtrees[key]
        
        postorder(root)
        return res
```

Time complexity: O(n)
Space complexity: O(n)

