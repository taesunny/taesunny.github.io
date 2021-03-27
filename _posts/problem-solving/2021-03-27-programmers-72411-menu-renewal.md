---
title:  "[Programmers] 메뉴 리뉴얼 (72413번, kakao blind recruitment 2021) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao, dfs]

toc: true
toc_sticky: true
 
date: 2021-03-27
last_modified_at: 2021-03-27
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/72411

---

# Solution

``` java
import java.util.*;

class Solution {
    public String[] solution(String[] orders, int[] course) {
        List<String> answer = new ArrayList<>();

        // string, count
        List<SortedMap<String, Integer>> possibleStringsArray = new ArrayList<>();
        int[] maxCount = new int[course[course.length - 1] + 1];

        for (int i = 0; i < course[course.length - 1] + 1; i++) {
            possibleStringsArray.add(new TreeMap<>());
        }

        for (int i = 0; i < orders.length; i++) {
            char[] temp = orders[i].toCharArray();
            Arrays.sort(temp);
            findString(new String(temp), possibleStringsArray, "", 0, maxCount, course[course.length - 1]);
        }

        for (int i = 0; i < course.length; i++) {
            SortedMap<String, Integer> possibleStrings = possibleStringsArray.get(course[i]);

            for (String string : possibleStrings.keySet()) {
                if (possibleStrings.get(string) == maxCount[course[i]] && possibleStrings.get(string) > 1) {
                    answer.add(string);
                }
            }
        }

        Collections.sort(answer);
        return answer.toArray(new String[answer.size()]);
    }

    private static void findString(String order, List<SortedMap<String, Integer>> possibleStringsArray,
            String previousString, int nextIndex, int[] maxCount, int maxLength) {
        if (nextIndex > order.length() - 1) {
            return;
        }

        findString(order, possibleStringsArray, previousString, nextIndex + 1, maxCount, maxLength);

        String newString = previousString + order.charAt(nextIndex);
        int newStringLength = newString.length();

        if (newStringLength > maxLength) {
            return;
        }

        if (newStringLength > 1) {
            if (possibleStringsArray.get(newStringLength).containsKey(newString)) {
                int newCount = possibleStringsArray.get(newStringLength).get(newString) + 1;

                possibleStringsArray.get(newStringLength).put(newString, newCount);
                if (maxCount[newStringLength] < newCount) {
                    maxCount[newStringLength] = newCount;
                }
            } else {
                possibleStringsArray.get(newStringLength).put(newString, 1);

                if (maxCount[newStringLength] == 0) {
                    maxCount[newStringLength] = 1;
                }
            }
        }

        findString(order, possibleStringsArray, newString, nextIndex + 1, maxCount, maxLength);
    }
}
```