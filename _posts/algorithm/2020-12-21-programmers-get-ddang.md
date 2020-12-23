---
title:  "[Programmers] 땅따먹기 - Java Solution"
excerpt: "Programmers 땅따먹기 Java Solution 입니다."

categories:
  - algorithm
tags:
  - [algorithm]

toc: true
toc_sticky: true

comments: true
 
date: 2020-12-21
last_modified_at: 2020-12-21
---
# Solution
```
class Solution {
    int solution(int[][] land) {
        int answer = 0;
        
        int x_length = land.length;
        int max = land[0][0];
        int nextMax = 0;

        for(int i=1; i<4; i++) {
            if(land[0][i] > max) {
                nextMax = max;
                max = land[0][i];
            } else if(land[0][i] > nextMax) {
                nextMax = land[0][i];
            }
        }
        
        if(x_length == 1) {
            return max;
        }
        
        for(int i=1; i<x_length; i++) {
            int tempMax = max;
            int tempNextMax = nextMax;
            
            for(int j=0; j<4; j++) {
                if(land[i-1][j] == tempMax) {
                    land[i][j] += tempNextMax;
                } else {
                    land[i][j] += tempMax;
                }
                
                if(j == 0) {
                    max = land[i][j];
                    nextMax = 0;
                } else {
                    if(land[i][j] > max) {
                        nextMax = max;
                        max = land[i][j];
                    } else {
                        if(land[i][j] > nextMax) {
                            nextMax = land[i][j];
                        }
                    }
                }
            }
        }

        return max;
    }
}
```