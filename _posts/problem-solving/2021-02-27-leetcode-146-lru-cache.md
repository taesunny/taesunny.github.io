---
title:  "[LeetCode] 146. LRU Cache - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, leetcode]

toc: true
toc_sticky: true
 
date: 2021-02-27
last_modified_at: 2021-02-27
---

# 문제
- https://leetcode.com/problems/lru-cache/

---

# Solution 1
```java
class LRUCache {
    int capacity;
    Map<Integer, Integer> data;
    Queue<Integer> history;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.data = new HashMap<>();
        this.history = new LinkedList<>();
    }

    public int get(int key) {
        if (this.data.containsKey(key)) {
            this.history.remove(key);
            this.history.add(key);

            return this.data.get(key);
        } else {
            return -1;
        }
    }

    public void put(int key, int value) {
        if (!this.data.containsKey(key)) {
            if (this.data.size() >= this.capacity) {
                int target = this.history.poll();
                this.history.remove(target);
                this.data.remove(target);
            }

            this.history.add(key);
        } else {
            this.history.remove(key);
            this.history.add(key);
        }

        this.data.put(key, value);
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */

```

## Result

![](/assets/img/problem-solving/2021-02-27-15-30-31.png)

---

# Todo
- cache 사용 기록을 업데이트 하는 작업을 O(n) 내에 할 수 있도록 수정하기