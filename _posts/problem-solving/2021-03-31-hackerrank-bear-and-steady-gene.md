---
title:  "[HackerRank] Bear and Steady Gene - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, hackerrank, sliding-window]

toc: true
toc_sticky: true
 
date: 2021-03-31
last_modified_at: 2021-03-31
---
# 문제
- https://www.hackerrank.com/challenges/bear-and-steady-gene/problem

---

# Solution

``` java
public class Solution {

    // Complete the steadyGene function below.
    static int aCount = 0;
    static int cCount = 0;
    static int tCount = 0;
    static int gCount = 0;
    static int geneLength = 0;
    static int eachLetterCount = 0;

    static boolean checkSteady() {
        if (aCount > eachLetterCount) {
            return false;
        }

        if (cCount > eachLetterCount) {
            return false;
        }

        if (tCount > eachLetterCount) {
            return false;
        }

        if (gCount > eachLetterCount) {
            return false;
        }

        return true;
    }

    // Complete the steadyGene function below.
    static int steadyGene(String gene) {
        geneLength = gene.length();
        eachLetterCount = geneLength / 4;
        char[] geneCharArray = gene.toCharArray();

        aCount = 0;
        cCount = 0;
        tCount = 0;
        gCount = 0;

        for (int i = 0; i < geneLength; i++) {
            char targetChar = geneCharArray[i];

            if (targetChar == 'A') {
                aCount++;
            } else if (targetChar == 'C') {
                cCount++;
            } else if (targetChar == 'T') {
                tCount++;
            } else if (targetChar == 'G') {
                gCount++;
            }
        }
        
        if(checkSteady()) {
            return 0;
        }

        int answer = 1_000_000_000;
        int front = -1;
        int tail = -1;
        boolean isStarted = false;

        for (front = -1; front < geneLength; front++) {
            while (front < geneLength - 1 && !checkSteady()) {
                front++;

                if (geneCharArray[front] == 'A') {
                    aCount--;
                } else if (geneCharArray[front] == 'C') {
                    cCount--;
                } else if (geneCharArray[front] == 'T') {
                    tCount--;
                } else if (geneCharArray[front] == 'G') {
                    gCount--;
                }

                if (isStarted) {
                    tail++;

                    if (geneCharArray[tail] == 'A') {
                        aCount++;
                    } else if (geneCharArray[tail] == 'C') {
                        cCount++;
                    } else if (geneCharArray[tail] == 'T') {
                        tCount++;
                    } else if (geneCharArray[tail] == 'G') {
                        gCount++;
                    }
                }

            }

            isStarted = true;
            front--;

            do {
                tail++;

                if (geneCharArray[tail] == 'A') {
                    aCount++;
                } else if (geneCharArray[tail] == 'C') {
                    cCount++;
                } else if (geneCharArray[tail] == 'T') {
                    tCount++;
                } else if (geneCharArray[tail] == 'G') {
                    gCount++;
                }
            } while (checkSteady());

            if (front == geneLength - 2) {
                if (!checkSteady()) {
                    break;
                }
            }

            int curWindowSize = front - tail + 2;
            answer = (answer > curWindowSize) ? curWindowSize : answer;
        }

        return answer;
    }

    // skipped code...
}
```