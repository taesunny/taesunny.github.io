---
title:  "[Programmers] 호텔 방 배정 (64063번) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao, level4]

toc: true
toc_sticky: true
 
date: 2021-02-03
last_modified_at: 2021-02-03
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/64063

---

# Timeout Solution

``` java
import java.util.*;

class Solution {
    public static long[] solution(long k, long[] room_number) {
        long[] answer = new long[room_number.length];
        int answerI = 0;
        LinkedList<Long> leftRooms = new LinkedList<>();
        
        for(long i=1; i<k+1; i++) {
            leftRooms.add(i);
        }
        
        for(int i=0; i<room_number.length; i++) {
            List<Long> toDeleteRoomList = new ArrayList<>();
            
            for(int j=0; j<leftRooms.size(); j++) {                
                if(leftRooms.get(j) >= room_number[i]) {
                    answer[answerI++] = leftRooms.get(j);
                    toDeleteRoomList.add(leftRooms.get(j));
                    i++;
                    
                    if(i>=room_number.length) {
                        break;
                    }

                    if(room_number[i-1] > room_number[i]) {
                        i--;
                        break;
                    }
                }
            }
            
            for(int j=0; j<toDeleteRoomList.size(); j++) {
                leftRooms.remove(toDeleteRoomList.get(j));
            }
        }
        
        return answer;
    }
}
```
---

# Solution

``` java
import java.util.*;

class Solution {
    Map<Long, Long> nextPossibleRooms = new HashMap<>();

    Long getPossibleRoom(long room) {
        if (!nextPossibleRooms.containsKey(room)) {
            return room;
        }

        long newPossibleRoom = getPossibleRoom(nextPossibleRooms.get(room));
        nextPossibleRooms.put(room, newPossibleRoom);

        return newPossibleRoom;
    }

    public long[] solution(long k, long[] room_number) {
        long[] answer = new long[room_number.length];
        int answerI = 0;

        for (int i = 0; i < room_number.length; i++) {
            if(!nextPossibleRooms.containsKey(room_number[i])) {
                answer[answerI++] = room_number[i];
                nextPossibleRooms.put(room_number[i], getPossibleRoom(room_number[i] + 1));
            } else {
                long nextRoom = getPossibleRoom(room_number[i]);
                answer[answerI++] = nextRoom;
                nextPossibleRooms.put(nextRoom, getPossibleRoom(nextRoom + 1));
            }
        }

        return answer;
    }
}
```


# 참고한 글

- https://yabmoons.tistory.com/486