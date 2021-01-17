---
title:  "[LeetCode] 438. Find All Anagrams in a String - Java Solution"
excerpt: "LeetCode 438. Find All Anagrams in a String Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, leetcode]

toc: true
toc_sticky: true
 
date: 2021-01-17
last_modified_at: 2020-01-17
---
# 문제
- https://leetcode.com/problems/find-all-anagrams-in-a-string/

---

# Solution
- Sliding Window 방식을 사용했다.
- 참고한 Discuss : https://leetcode.com/problems/find-all-anagrams-in-a-string/discuss/92059/O(n)-Sliding-Window-JAVA-Solution-Extremely-Detailed-Explanation

``` java
class Solution {    
    public List<Integer> findAnagrams(String s, String p) {
        
        List<Integer> answer = new ArrayList<>();
        
        if(s == null || s.length() == 0 || s.length() < p.length()) {
            return answer;
        }

        int freqData[] = new int['z' - 'a' + 1];
        
        for(int i=0; i<p.length(); i++) {
            freqData[p.charAt(i) - 'a']++;
        }
        
        int left = 0;
        int right = 0;
        
        int countToHit = p.length();
        
        for(right=0; right<p.length(); right++) {
            
            freqData[s.charAt(right) - 'a']--;
            
            if(freqData[s.charAt(right) - 'a'] >= 0) {
                countToHit--;
            }
        }
        
        if(countToHit == 0) {
            answer.add(left);
        }
        
        while(right < s.length()) {
            if(freqData[s.charAt(left) - 'a'] >= 0) {
                countToHit++;
            }
            
            freqData[s.charAt(left) - 'a']++;
            
            left++;
            
            if(countToHit == 0) {
                answer.add(left);
            }
            
            freqData[s.charAt(right) - 'a']--;
            
            if(freqData[s.charAt(right) - 'a'] >= 0) {
                countToHit--;
            }
            
            if(countToHit == 0) {
                answer.add(left);
            }
            
            right++;
        }

        return answer;
    }
}
```

---

# Result
- Runtime: 10 ms, faster than 57.74% of Java online submissions for Find All Anagrams in a String.
- Memory Usage: 44.8 MB, less than 6.27% of Java online submissions for Find All Anagrams in a String.