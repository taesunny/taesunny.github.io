---
title:  "[Programmers] 괄호 변환 (60058번) - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, programmers, kakao]

toc: true
toc_sticky: true
 
date: 2021-03-04
last_modified_at: 2021-03-04
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/60058

---

# Solution

``` java
class Solution {
    private boolean isRightString(String str) {
        if (str == null || "".equals(str)) {
            return false;
        }

        char[] arr = str.toCharArray();
        int balance = 0;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == '(') {
                balance++;
            } else {
                balance--;
            }

            if (balance < 0) {
                return false;
            }
        }

        return true;
    }

    public String solution(String str) {
        if (str == null || "".equals(str)) {
            return "";
        }

        char[] strArray = str.toCharArray();

        int balance = 0;
        int uLastIndex = 0;
        do {
            char cur = strArray[uLastIndex++];
            if (cur == '(') {
                balance--;
            } else {
                balance++;
            }
        } while (balance != 0);

        String u = str.substring(0, uLastIndex);
        String v = str.substring(uLastIndex, str.length());

        if (isRightString(u)) {
            String convertedV = solution(v);
            return u + convertedV;
        }

        String answer = "(" + solution(v) + ")";

        if (u.length() <= 2) {
            return answer;
        }

        char[] newUArray = u.substring(1, u.length() - 1).toCharArray();
        for (int i = 0; i < newUArray.length; i++) {
            answer += (newUArray[i] == ')' ? '(' : ')');
        }

        return answer;
    }
}
```
