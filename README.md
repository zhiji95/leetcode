# leetcode
A collection of my favorite Leetcode questions

### 248. Strobogrammatic Number III ###
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

       

### 395. Longest Substring with At Least K Repeating ### 
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

### 399. Evaluate Division ###
```python
class Solution(object):
    def calcEquation(self, equations, values, queries):
            """
            :type equations: List[List[str]]
            :type values: List[float]
            :type queries: List[List[str]]
            :rtype: List[float]
            """
            neighbors = {}
            for i, e in enumerate(equations):
                if not e[0] in neighbors:
                    neighbors[e[0]] = []
                if not e[1] in neighbors:
                    neighbors[e[1]] = []
                neighbors[e[0]].append((e[1], values[i]))
                neighbors[e[1]].append((e[0], 1/values[i]))

            def query(a, b, visited):
                if not a in neighbors: return 0
                if not b in neighbors: return 0
                if a == b: return 1.0
                visited.add(a)
                ans = 0
                for neighbor in neighbors[a]:
                    if neighbor[0] in visited: continue
                    elif neighbor[0] == b:
                        return neighbor[1]
                    ans = neighbor[1] * query(neighbor[0], b, visited)
                    if ans != 0:
                        return ans
                return 0

            out = []
            for q in queries:
                visited = set()
                out.append(query(q[0], q[1], visited))
            for i, o in enumerate(out):
                if o == 0: out[i] = -1.
            return out
```

### 562. Longest Line of Consecutive One in Matrix ###
[Link](https://leetcode.com/problems/longest-line-of-consecutive-one-in-matrix/)
```python 
class Solution(object):
    def longestLine(self, M):
        """
        :type M: List[List[int]]
        :rtype: int
        """
        if len(M) == 0:
            return 0
        
        dp = [[ [0, 0, 0, 0] for j in range(len(M[0]))] for i in range(len(M))]
        max_len = 0
        for row in range(len(M)):
            for col in range(len(M[0])):
                if M[row][col] == 1:
                    dp[row][col][0] = dp[row][col-1][0] + 1 if col > 0 else 1
                    dp[row][col][1] = dp[row-1][col][1] + 1 if row > 0 else 1
                    dp[row][col][2] = dp[row-1][col-1][2] + 1 if row > 0 and col > 0 else 1
                    dp[row][col][3] = dp[row-1][col+1][3] + 1 if row > 0 and col < len(M[0])-1 else 1

                max_len = max(max_len, dp[row][col][0], dp[row][col][1], dp[row][col][2], dp[row][col][3])
                
```

### 563. Binary Tree Tilt ###
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

### 662. Maximum Width of Binary Tree ###
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

### 683. K Empty Slots ###
##### xrange is faster than range  #####
``` 
range(n) creates a list containing all the integers 0..n-1. This is a problem if you do range(1000000), because you'll end up 
with a >4Mb list. xrange deals with this by returning an object that pretends to be a list, but just works out the number 
needed from the index asked for, and returns that.
However:
1. In python 3, range() does what xrange() used to do and xrange() does not exist. If you want to write code that will run on 
both Python 2 and Python 3, you can't use xrange().
2. range() can actually be faster in some cases - eg. if iterating over the same sequence multiple times.  xrange() has to 
reconstruct the integer object every time, but range() will have real integer objects. (It will always perform worse in terms
of memory however)
3. xrange() isn't usable in all cases where a real list is needed. For instance, it doesn't support slices, or any list 
methods.
```
```python
class Solution(object):      
    def kEmptySlots(self, flowers, k):
        """
        :type flowers: List[int]
        :type k: int
        :rtype: int
        """
        new = [0] * len(flowers)
        for i,n in enumerate(flowers, 1):
            new[n-1] = i
        result = float('inf')
        i = 0        
        while i < len(flowers) - (k + 1):
            right = new[i+k+1]
            left = new[i]
            for j in xrange(i+1, i + k + 1): 
        # The anwser will not be accepted if you use range instead of xrange, thus xrange is faster
                if new[j] < left or new[j]< right:
                    i = j
                    break
            else:
                result = min(result, max(left,right))
                i = i+k+1
        return result if result < float('inf') else -1
```

### 857. Minimum Cost to Hire K Workers ###
##### Max heap is min heap when pushing negative values. #####
``` python
class Solution(object):
    def mincostToHireWorkers(self, quality, wage, K):
        from fractions import Fraction
        workers = sorted((Fraction(w, q), q, w)
                         for q, w in zip(quality, wage))

        ans = float('inf')
        pool = []
        sumq = 0
        for ratio, q, w in workers:
            heapq.heappush(pool, -q)
            sumq += q

            if len(pool) > K:
                sumq += heapq.heappop(pool)

            if len(pool) == K:
                ans = min(ans, ratio * sumq)

        return float(ans)
```
