---
title:  "[Programmers] 문자열 압축 (60057번) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao]

toc: true
toc_sticky: true
 
date: 2021-02-13
last_modified_at: 2021-02-13
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/60057

---

# Solution

``` java
class Solution {
    static int getNumberLength(int number) {
        return Integer.toString(number).length();
    }
    
    public static int solution(String s) {
        int answer = s.length();
        
        if(s.length() == 1) {
            return answer;
        }
        
        int lengthData[] = new int[s.length()];
        
        for(int i=1; i<s.length()/2+1; i++) {

            int startIndex = 0;
            
            while(startIndex + i < s.length()+1) {
                int cnt = 1;
                String targetStr = s.substring(startIndex, startIndex + i);

                int nextStrStartIndex = startIndex + i;
                
                while(nextStrStartIndex + i < s.length() + 1) {
                    String nextStr = s.substring(nextStrStartIndex, nextStrStartIndex + i);
                    
                    if(targetStr.equals(nextStr)) {
                        cnt++;
                        nextStrStartIndex += i;
                    } else {
                        break;
                    }
                 }
                
                if(cnt > 1) {
                    lengthData[i] += getNumberLength(cnt);
                }

                lengthData[i] += i;
                startIndex = nextStrStartIndex;
            }
            
            int left = s.length() - startIndex;
            
            if(left > 0) {
                lengthData[i] += left;
            }
        }
        
        for(int i=1; i<s.length()/2+1; i++) {
            answer = (lengthData[i] < answer) ? lengthData[i] : answer;
        }
        
        return answer;
    }
}
```