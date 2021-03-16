---
title:  "[HackerRank] Special String Again - Java Solution"
excerpt: "HackerRank Special String Again Java Solution"

categories:
  - problem-solving
tags:
  - [algorithm, hackerrank, stringmanipulation]

toc: true
toc_sticky: true
 
date: 2021-01-26
last_modified_at: 2021-01-26
---
# 문제
- https://www.hackerrank.com/challenges/special-palindrome-again/problem

---

# Solution

문자열 s를 문자와 빈도 수의 객체로 압축한 후, 문자열을 지나며 special 한 문자열을 체크한다.

- [참고한 글 - alongtimeago.tistory](https://alongtimeago.tistory.com/397)

``` java
public class Solution {

    // Complete the countTriplets function below.
    static long substrCount(int n, String s) {
        
        List<int[]> encodedData = new ArrayList<>();

        long answer = 0;
        
        for(int i=0; i<s.length(); i++) {

            char ch = s.charAt(i);
            int tempCnt = 1;

            while(i+1 < s.length() && s.charAt(i+1) == ch) {
                i++;
                tempCnt++;
            }

            encodedData.add(new int[]{ch-'a', tempCnt});
        }
        
        for(int i=0; i<encodedData.size(); i++) {
            int tempLen = encodedData.get(i)[1];
            answer += tempLen*(tempLen + 1) / 2;
        }

        for(int i=1; i<encodedData.size()-1; i++) {
            int leftChar = encodedData.get(i-1)[0];
            int rightChar = encodedData.get(i+1)[0];
            int centerLen = encodedData.get(i)[1];

            if(leftChar == rightChar && centerLen == 1) {
                int leftLen = encodedData.get(i-1)[1];
                int rightLen = encodedData.get(i+1)[1];

                answer += Math.min(leftLen, rightLen);
            }
        }

        return answer;
    }

    // skipped code
}
```

---


# 하지 말아야할 방식

엄청난 공간낭비와 timeout을 확인 할 수 있다.

``` java
public class Solution {

    // Complete the substrCount function below.
    static long substrCount(int n, String s) {
        
        boolean[][] data = new boolean[s.length() + 1][s.length()];

        long answer = 0;
        
        for(int i=0; i<s.length(); i++) {
            data[1][i] = true;
            answer++;
        }
        
        for(int len=2; len<s.length()+1; len++) {
            for(int walker = 0; walker<s.length()-len+1; walker++) {
                
                int halfLen = len / 2;
                
                if(len%2 == 0) {
                    if(data[halfLen][walker] && data[halfLen][walker+halfLen] && s.charAt(walker) == s.charAt(walker + halfLen)) {
                        data[len][walker] = true;
                        answer++;
                    }
                } else {
                    if(data[halfLen][walker] && data[halfLen][walker+halfLen+1] && s.charAt(walker) == s.charAt(walker+halfLen+1)) {
                        if(s.charAt(walker) == s.charAt(walker+halfLen)) {
                            data[len][walker] = true;
                        }
                        
                        answer++;
                    }
                }

                System.out.println("len : " + len + ", walker : " + walker + ", answer : " + answer);
                printData(data);
            }
        }

        return answer;
    }

    // skipped code
}
```