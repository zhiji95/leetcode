# leetcode
A collection of my favorite Leetcode questions


**395. Longest Substring with At Least K Repeating **

`    

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
        return max(result, self.longestSubstring(temp, k))`
```
