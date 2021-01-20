---
title:  "[HackerRank][Stack and Queues] Min Max Riddle - Java Solution"
excerpt: "HackerRank Min Max Riddle Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, hackerrank, stack, queue]

toc: true
toc_sticky: true
 
date: 2021-01-19
last_modified_at: 2020-01-20
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
# Solution
- https://sungjun221.github.io/algorithm/Min-Max-Riddle/의 내용을 참고해서 풀었다.
    - 해당 설명을 보고 본인의 스타일로 푸니 inverted_windows에서, key가 작은데도 key가 큰 경우 보다 inverted_windows가 작게 나와서 마지막에 보정을 추가했다.

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
        long[] answer = new long[arr.length];
        
        // number, maximum window size that the number can minimum in the window
        SortedMap<Long, Integer> maxWindowOfNumberMap = new TreeMap<>();

        for(int i=0; i<arr.length; i++) {
            long number = arr[i];
            int left = i-1;
            int right = i+1;
            int tempMaxWindowSize = 1;
            
            while(left >= 0) {
                if(arr[left] >= number) {
                    left--;
                    tempMaxWindowSize++;
                } else {
                    break;
                }
            }
            
            while(right < arr.length) {
                if(arr[right] >= number) {
                    right++;
                    tempMaxWindowSize++;
                } else {
                    break;
                }
            }
            
            if(maxWindowOfNumberMap.containsKey(number)) {
                int previousWindowSize = maxWindowOfNumberMap.get(number);
                if(tempMaxWindowSize > previousWindowSize) {
                    maxWindowOfNumberMap.put(number, tempMaxWindowSize);
                }
            } else {
                maxWindowOfNumberMap.put(number, tempMaxWindowSize);
            }
        }

        // window size, maxNumber
        Map<Integer, Long> maxNumberOfWindowMap = new HashMap<>();
        
        for(Long key : maxWindowOfNumberMap.keySet()) {
            Integer windowSize = maxWindowOfNumberMap.get(key);
            
            if(!maxNumberOfWindowMap.containsKey(windowSize)) {
                maxNumberOfWindowMap.put(windowSize, key);
            } else {
                Long previousNumber = maxNumberOfWindowMap.get(windowSize);
                
                if(previousNumber < key) {
                    maxNumberOfWindowMap.put(windowSize, key);
                }
            }
        }
        
        for(int i=0; i<answer.length; i++) {
            int windowSize = i+1;
            
            if(maxNumberOfWindowMap.containsKey(windowSize)) {
                answer[i] = maxNumberOfWindowMap.get(windowSize);
            }
        }
        
        for(int i=answer.length-1; i>=0; i--) {            
            if(answer[i] == 0) {
                answer[i] = answer[i+1];
            }
            
            if(i!=answer.length-1 && answer[i] < answer[i+1]) {
                answer[i] = answer[i+1];
            }
        }
        
        return answer;
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