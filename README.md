# leetcode
A collection of my favorite Leetcode questions

```248. Strobogrammatic Number III```
```python
class Solution:
    res = 0
    def strobogrammaticInRange(self, low, high):
        """
        :type low: str
        :type high: str
        :rtype: int
        """
        for length in range(len(low),len(high)+1):
            self.dfsHelper(low,high, length, "")
            self.dfsHelper(low,high, length, "1")
            self.dfsHelper(low,high, length, "8")
            self.dfsHelper(low,high, length, "0")
        return self.res
            
        
        
    def dfsHelper(self,low,high,length,path):
        if len(path) > length:
            return
        if len(path) == length:
            if len(path) != 1 and path[0] == '0':
                return
            else:
                if int(path) >= int(low) and int(path) <= int(high):
                    self.res +=1
                return
        self.dfsHelper(low,high, length, '0'+path+'0')
        self.dfsHelper(low,high, length, '6'+path+'9')
        self.dfsHelper(low,high, length, '9'+path+'6')
        self.dfsHelper(low,high, length, '8'+path+'8')
        self.dfsHelper(low,high, length, '1'+path+'1')
```

       

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

```662. Maximum Width of Binary Tree```
```python
class Solution:
    def widthOfBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        else:
            root.val = 1
            dic = {}
            self.traverse(root, 0,  dic)
            
                    
            return max([max(l) - min(l) for l in dic.values()]) + 1
            

    def traverse(self, root, depth, dic):
        if not root:
            return 
        else:
            if depth in dic.keys():
                if root.val not in dic[depth]:
                    dic[depth].append(root.val)
            else:
                dic[depth] = [root.val]
                
            if root.left:
                root.left.val = 2 * root.val - 1
                self.traverse(root.left, depth + 1, dic)
            if root.right:
                root.right.val = 2 * root.val
                self.traverse(root.right, depth + 1, dic)

```
