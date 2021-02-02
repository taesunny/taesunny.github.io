---
title:  "[LeetCode] 14. Longest Common Prefix - Java Solution"
excerpt: "LeetCode 14. Longest Common Prefix Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, leetcode, string]

toc: true
toc_sticky: true
 
date: 2021-01-18
last_modified_at: 2021-01-18
---
# 문제
- https://leetcode.com/problems/longest-common-prefix/

---

# Solution

``` java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        
        String answer = "";
        
        if(strs == null || strs.length == 0) {
            return "";
        }
        
        if(strs.length < 2) {
            return strs[0];
        }
        
        int minlength = 201;
        
        for(String str : strs) {
            int len = str.length();
            minlength = (len < minlength) ? len : minlength;
        }
        
        boolean isSameChar = true;
        
        for(int i=0; i<minlength && isSameChar; i++) {
            char targetChar = strs[0].charAt(i);
            isSameChar = true;
            
            for(int j=1; j<strs.length; j++) {
                char tempChar = strs[j].charAt(i);
                
                if(tempChar != targetChar) {
                    isSameChar = false;
                    break;
                }
            }
            
            if(isSameChar) {
                answer += targetChar;
            }
        }
        
        return answer;
    }
}
```

---

# Result
- Runtime: 7 ms, faster than 20.06% of Java online submissions for Longest Common Prefix.
- Memory Usage: 39.4 MB, less than 5.47% of Java online submissions for Longest Common Prefix.