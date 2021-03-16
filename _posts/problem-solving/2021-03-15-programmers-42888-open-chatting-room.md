---
title:  "[Programmers] 오픈채팅방 (42888번, kakao blind recruitment 2019) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao]

toc: true
toc_sticky: true
 
date: 2021-03-15
last_modified_at: 2021-03-15
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/42888

---

# Solution

``` java
import java.util.*;

class Solution {
    public String[] solution(String[] record) {
        List<String[]> history = new ArrayList<>();
        Map<String, String> nicknames = new HashMap<>();

        for (int i = 0; i < record.length; i++) {
            String[] splitted = record[i].split(" ");

            String act = splitted[0];
            String id = splitted[1];

            if (!"Leave".equals(act)) {
                String nickname = splitted[2];
                nicknames.put(id, nickname);
            }

            if ("Change".equals(act)) {
                continue;
            }

            String[] element = new String[] { act, id };
            history.add(element);
        }

        String[] answer = new String[history.size()];

        for (int i = 0; i < history.size(); i++) {
            String result = "";
            String[] data = history.get(i);
            if ("Enter".equals(data[0])) {
                result += nicknames.get(data[1]) + "님이 들어왔습니다.";
            } else {
                result += nicknames.get(data[1]) + "님이 나갔습니다.";
            }

            answer[i] = result;
        }

        return answer;
    }
}
```
