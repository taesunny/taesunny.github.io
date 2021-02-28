---
title:  "[HackerRank] Bigger is Greater - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, hackerrank]

toc: true
toc_sticky: true
 
date: 2021-02-28
last_modified_at: 2020-02-28
---

# 문제
- https://www.hackerrank.com/challenges/bigger-is-greater/problem

---

# Solution

``` java
public class Solution {

    // Complete the biggerIsGreater function below.
    static String biggerIsGreater(String w) {
        final String noAnswer = "no answer";

        int start = w.length() - 2;
        char[] wArray = w.toCharArray();

        while (start >= 0) {

            int toSwap = -1;

            for (int i = start + 1; i < w.length(); i++) {
                if (wArray[i] > wArray[start]) {
                    if(toSwap == -1) {
                        toSwap = i;
                    } else {
                        if(wArray[toSwap] > wArray[i]) {
                            toSwap = i;
                        }
                    } 
                }
            }

            if(toSwap != -1) {
                char temp = wArray[toSwap];
                wArray[toSwap] = wArray[start];
                wArray[start] = temp;

                Arrays.sort(wArray, start + 1, w.length());

                return new String(wArray);
            }

            start--;
        }

        return noAnswer;
    }

    // skipped code
}
```