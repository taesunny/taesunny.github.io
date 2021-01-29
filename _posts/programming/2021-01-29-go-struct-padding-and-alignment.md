---
title:  "[Go] Go struct Padding and Alignment"
excerpt: "About Go struct Padding and Alignment"

categories:
  - programming
tags:
  - [go, golang, struct]

toc: true
toc_sticky: true
 
date: 2021-01-29
last_modified_at: 2021-01-29
---

{:refdef: style="text-align: center;"}
![Go Logo](/assets/img/programming/2021-01-15-09-57-39.png)
{: refdef}

Go struct의 메모리 Padding과 Alignment 때문에, struct의 메모리를 줄이기 위해서는 `Go 에서 struct를 정의할 때, 필드들의 크기를 정렬해야한다.`

---

## Alignment and Padding

예를 들어 아래와 같이 struct type sunny를 정의 할 경우, 7bytes를 차지할 것으로 예상하지만 실제로는 8bytes를 차지한다.

```
type sunny struct {
  name   float32 // 4bytes
  age    int16 // 2 bytes
  male   bool // 1 byte
}
```

Go는 'Alignment' 단위로 memory로 부터 데이터를 읽는 것이 효과적이기 때문에 'Alignment' 크기에 맞지 않는 필드의 경우 'Padding'을 두어, 전체 struct의 크기를 조절한다.

- 규칙 1: 선언된 순서대로 메모리에 저장된다.
- 규칙 2: 필드가 최소로 차지하는 기본 크기는 '2bytes'이다. 예) 1byte의 bool type은 기본적으로 2bytes를 차지하도록 1byte padding이 자동 추가 된다.
- 규칙 3: 가장 큰 크기의 field 크기를 alignment라는 단위로 사용해, 나머지 크기가 빌 경우 padding이 추가된다.

### 예시
1. 총 8 bytes 크기

    ```go
    type sunny struct { // alignment 4
        name   float32 // 4 bytes
        age    int16 // 2 bytes
        male   bool // 1 byte
        // padding 2 bytes
    }
    ```

2.  총 12 bytes 크기

    ```go
    type sunny struct { // aligntment 4
        age    int16 // 2 bytes
        // padding 2 bytes
        name   float32 // 4 bytes
        male   bool // 1 byte
        // padding 3 bytes
    }
    ```
3. 총 8 bytes 크기

    ```go
    type sunny struct { // aligntment 4
        male   bool // 1 byte
        // padding 1 byte
        age    int16 // 2 bytes
        name   float32 // 4 bytes
    }
    ```

4. 총 16 bytes 크기

    ```go
    type sunny struct { // aligntment 8
        name   int64 // 8 bytes
        male   bool // 1 byte
        // padding 1 byte
        age    int16 // 2 bytes
        // padding 4 bytes
    }
    ```

## 결론

- struct 필드들의 크기 순서가 정렬되어 있어야 데이터를 적게 차지한다.
- `Go 에서 struct를 정의할 때, 가장 큰 크기의 필드 부터 정의해야한다.` or `적당히 가독성이 좋도록 배열한다.`라는 의견의 글이 있다.


---

## Source
- https://github.com/ardanlabs/gotraining-studyguide/
- https://johngrib.github.io/wiki/golang-struct-padding/