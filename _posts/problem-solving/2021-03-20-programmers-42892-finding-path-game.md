---
title:  "[Programmers] 길 찾기 게임 (42892번, kakao blind recruitment 2019) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao]

toc: true
toc_sticky: true
 
date: 2021-03-20
last_modified_at: 2021-03-20
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/42892

---

# Solution

``` java
import java.util.*;

class Solution {
    class Node {
        int id;
        int x;
        int y;
        Node left;
        Node right;

        Node(int id, int x, int y) {
            this.id = id;
            this.x = x;
            this.y = y;
        }
    }
    
    public int[][] solution(int[][] nodeinfo) {
        List<Node> data = new ArrayList<>();

        for (int i = 0; i < nodeinfo.length; i++) {
            data.add(new Node(i + 1, nodeinfo[i][0], nodeinfo[i][1]));
        }

        data.sort((a, b) -> (a.y == b.y) ? (a.x - b.x) : (b.y - a.y));

        Node root = data.get(0);
        for (int i = 1; i < data.size(); i++) {
            insertNode(root, data.get(i));
        }

        int[] preOrder = new int[data.size()];
        getPreOrder(root, preOrder);

        int[] postOrder = new int[data.size()];
        getPostOrder(root, postOrder);

        int[][] answer = { preOrder, postOrder };
        return answer;
    }

    void insertNode(Node root, Node target) {
        if (target.x < root.x) {
            if (root.left == null) {
                root.left = target;
            } else {
                insertNode(root.left, target);
            }
        } else {
            if (root.right == null) {
                root.right = target;
            } else {
                insertNode(root.right, target);
            }
        }
    }

    int preorderIdx = 0;

    void getPreOrder(Node root, int[] container) {
        if (root == null) {
            return;
        }

        container[preorderIdx++] = root.id;
        getPreOrder(root.left, container);
        getPreOrder(root.right, container);
    }

    int postorderIdx = 0;

    void getPostOrder(Node root, int[] container) {
        if (root == null) {
            return;
        }

        getPostOrder(root.left, container);
        getPostOrder(root.right, container);
        container[postorderIdx++] = root.id;
    }
}
```
