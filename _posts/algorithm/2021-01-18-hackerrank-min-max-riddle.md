---
title:  "[HackerRank][Stack and Queues] Min Max Riddle - Java Solution"
excerpt: "HackerRank Min Max Riddle Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, hackerrank, stack, queue]

toc: true
toc_sticky: true
 
date: 2021-01-18
last_modified_at: 2020-01-18
---
# 문제
- https://www.hackerrank.com/challenges/min-max-riddle/problem

---

# Timeout Solution

``` java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

    // Complete the riddle function below.
    static long[] riddle(long[] arr) {
        // complete this function
        List<Long> answer = new ArrayList<>();
        long[] temp = new long[arr.length];

        long maximumOfAllWindows = arr[0];
        temp[0] = arr[0];
        
        for(int i=1; i<arr.length; i++) {
            temp[i] = arr[i];
            maximumOfAllWindows = (maximumOfAllWindows < arr[i]) ? arr[i] : maximumOfAllWindows;
        }
        
        answer.add(maximumOfAllWindows);
        
        for(int windowSize=2; windowSize<arr.length+1; windowSize++) {            
            for(int i=0; i<arr.length-windowSize+1; i++) {               
                long target = arr[i+windowSize-1];
                temp[i] = (temp[i] > target) ? target : temp[i];
               
                if(i==0) {
                    maximumOfAllWindows = temp[i];
                } else {
                    maximumOfAllWindows = (maximumOfAllWindows < temp[i]) ? temp[i] : maximumOfAllWindows;
                }
            }
            
            answer.add(maximumOfAllWindows);
        }
        
        long[] answerArr = new long[answer.size()];
        
        for(int i=0; i<answerArr.length; i++) {
            answerArr[i] = answer.get(i);
        }
        
        return answerArr;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        long[] arr = new long[n];

        String[] arrItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < n; i++) {
            long arrItem = Long.parseLong(arrItems[i]);
            arr[i] = arrItem;
        }

        long[] res = riddle(arr);

        for (int i = 0; i < res.length; i++) {
            bufferedWriter.write(String.valueOf(res[i]));

            if (i != res.length - 1) {
                bufferedWriter.write(" ");
            }
        }

        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}

```

---

# Result
- Runtime: 7 ms, faster than 20.06% of Java online submissions for Longest Common Prefix.
- Memory Usage: 39.4 MB, less than 5.47% of Java online submissions for Longest Common Prefix.