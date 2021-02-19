---
title:  "How to update windows10 hosts file"
excerpt: ""

categories:
  - howto
tags:
  - [howto, windows10, hosts]

toc: true
toc_sticky: true
 
date: 2021-02-19
last_modified_at: 2021-02-19
---

Windows에서도 linux 에서처럼 hosts 파일을 수정하여 IP와 Host name을 등록해 사용할 수 있다.

## Hosts File Path
Windows10에서 Host 파일의 위치는 다음과 같다.

```
C:\Windows\System32\drivers\etc\hosts
```

## 관리자권한으로 hosts file 수정하기
관리자권한으로 메모장을 열어 해당파일을 수정할 수 있다.

- 메모장을 관리자권한으로 실행

    {:refdef: style="text-align: center;"}
    ![메모장 실쟁](/assets/img/howto/2021-02-19-13-49-21.png)
    {: refdef}

- 열기 메뉴를 통해 hosts 파일을 열어준다.

    {:refdef: style="text-align: center;"}
    ![hosts file open](/assets/img/howto/2021-02-19-13-53-00.png)
    {: refdef}

- 해당 파일에 IP와 Host Name을 추가 후 저장한다.

    ```
    10.0.0.7    my-server
    ```

## Test

등록한 Host Name을 사용해 hosts file에 등록한 IP로의 접근이 가능한지 확인한다.

{:refdef: style="text-align: center;"}
![hosts file open](/assets/img/howto/2021-02-19-13-55-19.png)
{: refdef}