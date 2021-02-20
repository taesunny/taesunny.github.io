---
title:  "Sorting Algorithms"
excerpt: ""

categories:
  - analysis
tags:
  - [sorting, algorithm]

toc: true
toc_sticky: true
 
date: 2021-01-30
last_modified_at: 2021-02-18
---

# 정렬

## Points

- 정렬할 데이터의 양
- 데이터와 메모리 (메모리에 다 올릴 수 있는지)
- 데이터가 정렬된 정도
- 필요한 추가 메모리의 양
- 상대 위치 보존 여부 - 보존되면 → 안정적인 알고리즘

## 종류

### 선택 정렬 (Selection Sorting)

- 가장 단순
- 첫 원소부터 배열 전체를 훑으면서 가장 작은 키를 앞쪽에 있는 원소들과 교체하면서 정렬
- 최선, 평균, 최악의 경우 O(n^2)
- 원소를 맞바꾸는 횟수가 최대 n-1 → 데이터 원소 이동이 느릴 경우 장점
- 제자리 정렬 알고리즘
- 일반적으로 불안정

#### 예제

```java
    private static void swap(int[] data, int index1, int index2) {
        int temp = data[index1];
        data[index1] = data[index2];
        data[index2] = temp;
    }

    private static void selectionSort(int[] data) {
        int pointer = 0;

        while (pointer < data.length - 1) { // no need to check the last element
            int minIndex = pointer;

            // find min value's index
            for (int i = pointer + 1; i < data.length; i++) {
                if (data[minIndex] > data[i]) {
                    minIndex = i;
                }
            }

            if (pointer != minIndex) {
                swap(data, pointer, minIndex);
            }

            pointer++;
        }
    }
```

### 삽입 정렬 (Insertion Sorting)

- 간단한 알고리즘
- 한 번에 한 원소 씩 이미 정렬된 다른 원소들과 비교하면서 적절한 위치에 삽입
- 최선(리스트가 이미 정렬 되어 있을 경우) O(n)
- 최저, 평균 실행시간이 모두 O(n^2)
- 이미 정렬된 리스트에 새 원소 추가 시, 데이터 크기가 작을 때 효율적
- 무작위의 데이터에서 안좋음
- 보통 안정적인 제자리 정렬

#### 예제

```java
    private static void insertionSort(int[] data) {
        for (int i = 1; i < data.length; i++) {
            int toInsert = data[i];

            int targetToMove = 0;
            for (targetToMove = i - 1; targetToMove >= 0 && data[targetToMove] > toInsert; targetToMove--) {
                data[targetToMove + 1] = data[targetToMove];
            }

            data[targetToMove + 1] = toInsert;
        }
    }
```

### 퀵 정렬 (Quick Sorting)

- 기준이 될 피벗 값을 고르고, 왼쪽에 집합에 피벗 보다 작은 값들을, 오른쪽 집합에 피벗 보다 큰 값들을 위치 시킴, 더 이상 쪼갤 부분집합이 없을 때까지 각 집합들을 재귀적으로 정렬
- 피벗값: 전체 데이터를 절반씩으로 쪼갤 수 있어야 가장 이상적
- 최선의 경우: 절반씩 나누는 연산 → log(n), n개의 원소의 경우 O(n log(n)) 복잡도
- 최악의 경우(피벗을 잘못 고르면-데이터의 최솟값이나 최대값을 고르는 경우): 재귀 호출 횟수 O(n), 최악의 경우 복잡도 O(n^2)
- 최선, 평균 복잡도: O(n log(n))
- 불안정 정렬

#### 예제

```java
    private static void quickSort(int[] data) {
        quickSort(data, 0, data.length - 1);
    }

    private static void quickSort(int[] data, int start, int end) {
        if (start < end) {
            int pivotPosition = partition(data, start, end);

            quickSort(data, start, pivotPosition - 1);
            quickSort(data, pivotPosition + 1, end);
        }
    }

    private static int partition(int[] data, int start, int end) {
        int pivot = data[start];
        int left = start;
        int right = end + 1;
        int temp = 0;

        do {
            do {
                left++;
            } while (left <= end && data[left] < pivot);

            do {
                right--;
            } while (right >= start && data[right] > pivot);

            if (left < right) {
                temp = data[left];
                data[left] = data[right];
                data[right] = temp;
            }
        } while (left < right);

        temp = data[start];
        data[start] = data[right];
        data[right] = temp;

        return right;
    }
```

### 병합 정렬 (Merge Sorting)

- 데이터 집합을 둘 이상의 부분집합으로 나누고, 각 부분집합을 정렬하고, 부분집합들을 합치며 정렬
- Divide and Conquer(분할 정복형) 방식
- 데이터 집합이 메모리에 한번에 올리기 어려울 때 쓰기 좋음
- 최고, 최저, 평균 실행시간이 모두 O(n log(n))
- O(n)의 추가 메모리 필요
- 안정적
- 제자리 정렬 알고리즘 x

#### 예제

```java
    // main method
    private static void mergeSort(int[] data) {
        int[] temp = new int[data.length];

        mergeSort(data, temp, 0, data.length - 1);
    }

    private static void mergeSort(int[] data, int[] temp, int start, int end) {
        if (start < end) {
            int leftEnd = (start + end) / 2;
            mergeSort(data, temp, start, leftEnd);
            mergeSort(data, temp, leftEnd + 1, end);
            merge(data, temp, start, leftEnd, end);
        }
    }

    private static void merge(int[] data, int[] temp, int start, int leftEnd, int end) {
        int iLeft = start;
        int iRight = leftEnd + 1;
        int iResult = start;

        while (iLeft <= leftEnd && iRight <= end) {
            if (data[iLeft] <= data[iRight]) {
                temp[iResult++] = data[iLeft++];
            } else {
                temp[iResult++] = data[iRight++];
            }
        }

        while (iLeft <= leftEnd) {
            temp[iResult++] = data[iLeft++];
        }

        // no need to move right side left elements
        // while (iRight <= end) {
        //     temp[iResult++] = data[iRight++];
        // }

        for (int i = start; i < iRight; i++) {
            data[i] = temp[i];
        }
    }
```

### 거품 정렬 (Bubble Sorting)

- 구현이 단순
- 데이터의 처음부터 인접한 원소를 비교하면서 순서가 맞지 않을 경우 위치를 교체, 순서가 맞는 경우 다시 처음부터 비교하는 로직을 반복
- 평균, 최악 O(n^2), 최선의 경우 O(n)
- 가장 느림, 거이 안쓰임

## 예제

```java
    private static void bubbleSort(int[] data) {
        int last = data.length - 1;
        int temp = 0;

        while (last > 0) {
            for (int i = 0; i < last; i++) {
                if (data[i] > data[i + 1]) {
                    temp = data[i];
                    data[i] = data[i + 1];
                    data[i + 1] = temp;
                }
            }

            last--;
        }
    }
```

### 힙 정렬 (Heap Sorting)

- 최대 힙 트리나 최소 힙 트리로 정렬
- 항상 O(n log(n)) 시간 복잡도, 공간 복잡도 O(1)
- 힙
    - 트리의 루트부터 가로 방향으로 1부터 인덱싱 (왼쪽 자식의 인덱싱은 부모의 두배, 오른쪽 자식의 인덱싱은 왼쪽 자식의 인덱스 더하기 1
    - 데이터 삽입 시, 가장 마지막 인덱스에 저장, 부모들과 크기 비교하면서 교환
    - 데이터(가장 크거나 가장 작은 값, 루트 값) 삭제 시, 해당 자리에 힙의 마지막 노드를 위치시킴, 자식들을 비교하면서 교환

---

# Source

- [https://ko.wikipedia.org/wiki/거품_정렬](https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC)
- [https://ko.wikipedia.org/wiki/힙_정렬](https://ko.wikipedia.org/wiki/%ED%9E%99_%EC%A0%95%EB%A0%AC)
- [https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html)
- 책, 프로그래밍 면접 이렇게 준비한다 - 한빛미디어