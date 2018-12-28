# leetcode
A collection of my favorite Leetcode questions


       

```395. Longest Substring with At Least K Repeating```
```python
 class Solution:
    def longestSubstring(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        if not s:
            return 0
        dic = {}
        for c in s:
            if c in dic.keys():
                dic[c] += 1
            else:
                dic[c] = 1   
        if (dic.values()) and min(dic.values()) >= k:
            return len(s)
        temp = ''
        result = 0
        for c in s:
            if dic[c] < k:
                result = max(result, self.longestSubstring(temp, k))
                temp = ''
            else:
                temp += c
        return max(result, self.longestSubstring(temp, k))
```

```563. Binary Tree Tilt```
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def findTilt(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        return self.traverse(root)[1]
    def traverse(self, root):
        if not root:
            return [0,0]
        else:
            left = self.traverse(root.left)
            right = self.traverse(root.right)
            summation = left[0] + right[0] + root.val
            tilt = left[1] + right[1] + abs(left[0] - right[0])
            return [summation, tilt]
```
