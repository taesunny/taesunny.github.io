---
title:  "HTTP & SSL(TLS) 정리"
excerpt: "About HTTP and SSL(TLS)"

categories:
  - analysis
tags:
  - [https, ssl, http, analysis]

toc: true
toc_sticky: true
 
date: 2020-12-14
last_modified_at: 2020-12-14
---
# HTTP & SSL(TLS)
## HTTPS VS HTTP
### HTTP
- Hypertext Transfer Protocol의 약자
- HTML을 전송하기 위한 통신규약
- 내용을 암호화 하지 않아 메세지 감청이 쉬움

### HTTPS
- 마지막의 S는 Over Secure Socket Layer의 약자
- 보안이 강화된 HTTP
- 내용을 암호화 하여 전송

## HTTP & SSL
- SSL 프로토콜 위에서 HTTPS가 돌아간다.

## SSL & TLS
- 둘이 동일
- 넷스케이프에 의해 SSL 발명
- IETF에서 관리되면서 TLS로 이름이 바뀜
- TLS 1.0이 SSL 3.0을 계승

## SSL 디지털 인증서
- 클라이언트와 서버간의 통신을 제3자가 보증해주는 전자화된 문서
- 클라이언트가 접속하려는 서버가 신뢰 할 수 있는 서버인지를 판단 가능
- 사용 과정
    - 클라이언트가 서버에 접속
    - 서버가 클라이언트에게 인증서 정보 전달
    - 클라이언트가 인증서 정보가 신뢰할 수 있는지 검증 후 정보 교환

## SSL에서의 암호화 기법
- 키
    - 암호화(암호를 만드는 것, <-> 복호화)를 할 때 사용하는 일종의 비밀번호

### 대칭키
- 동일한 키로 암호화와 복호화를 같이 할 수 있는 방식
- 단점
    - 대칭키 전달이 어렵다.
    - 대칭키 유출 시 위험
- openssl 암호화 예제
    - 옵션 `enc -e -des3` : des3 방식으로 암호화
    - 옵션 `-in plaintext.txt -out ciphertext.bin` : plaintext.txt 암호화 결과를 ciphertext.bin 파일에 저장
```
echo 'my txt' > plaintext.txt;
openssl enc -e -des3 -salt -in plaintext.txt -out ciphertext.bin;
```
- openssl 복호화 예제
    - 옵션 `enc -d` : ciphertext.bin 파일을 plaintext2.txt 파일로 복호화
```
openssl enc -d -des3 -in ciphertext.bin -out plaintext2.txt;
```

### 공개키
- 대칭키의 유출 시 위험한 문제를 해결하기 위해 나옴
- 두 개의 키가 존재
    - A키로 암호화를 하면 B키로 복호화 가능
    - B키로 암호화하면 A키로 복호화 가능
    - 두개의 키중 하나를 비공개키(private key)
    - 다른 키를 공개키(public key)로 지정
    - 공개키를 타인에게 제공
- 공개키가 유출 되어도 비공개키를 모르면 복호화가 불가능하여 안전
- 기본 사용 과정
    - 비공개키의 소유자가 비공개키를 이용해서 정보를 암호화 한 후, 공개키와 함께 암호화된 정보를 전송
    - 공개키를 획득한 사람이 공개키를 이용해서 암호화된 정보를 복호화
- 공개키 분실시 위험, but, 공개키로 복호화 가능하다는 것이 데이터를 제공한 사람의 신원을 보장할 수 있다는 장점이 있다. -> 전자 서명

#### 예제
- private.pem이라는 이름의 키를 생성
    - 길이 1024bit, 길수록 안전
```
openssl genrsa -out private.pem 1024;
```
- private.pem이라는 이름의 비공개키에 대한 public.pem이라는 이름의 공개키를 생성
```
openssl rsa -in private.pem -out public.pem -outform PEM -pubout;
```
-  file.txt의 내용을 RSA방식으로 암호화한 file.ssl이라는 이름의 파일을 생성
```
openssl rsautl -encrypt -inkey public.pem -pubin -in file.txt -out file.ssl;
```
-  file.ssl을 복호화, 결과를 decrypted.txt로 생성
```
openssl rsautl -decrypt -inkey private.pem -in file.ssl -out decrypted.txt
```

## SSL 인증서
### 기능
- 클라이언트가 접속한 서버가 신뢰 할 수 있는 서버임을 보장
- SSL 통신에 사용할 공개키를 클라이언트에게 제공


### CA
- 인증서: 클라이언트가 접속한 서버가 의도한 서버가 맞는지를 보장
- 인증 기관: CA(Certificate authority), Root Certificate
- CA를 통해서 인증서를 구입해야함
- 사설 인증기관
    - 개발용으로 직접 CA 역할을 할 수 있음
    - 공인된 인증서가 아니므로 브라우저가 경고 출력


### 인증서 내용
- 공개키 방식으로 암호화
1. 서비스의 정보
    - 인증서 발급 CA
    - 서비스 도메인
2. 서버 측 공개키 정보
    - 공개키 내용
    - 공개키 암호화 방법


### CA 리스트
- 브라우저가 내부적으로 CA의 리스트를 미리 파악
- CA의 공개키도 브라우저가 알고 있다.


### SSL 인증서가 신뢰 할 수 있는 서버 보장하는 방법
1. 클라이언트(웹 브라우저)가 서버 접속 시 서버가 인증서 제공
2. 브라우저가 CA 목록 확인
3. 해당 CA의 공개키를 이용해서 인증서 복호화, 성공
    - 해당 인증서가, 해당 CA의 비공개키에 의해 암호화 된 것을 알 수 있다.
    - 신뢰 할 수 있는 사이트


### SSL 동작 방법
- 결론: 공개키와 대칭키를 혼합해서 사용
    - 이유: `공개키 방식이 많은 컴퓨팅 파워를 사용하기 때문`
- 클라이언트, 서버가 주고 받는 실제 정보 -> `대칭키` 방식으로 암호화, 복호화
- `대칭키`는 공개키 방식으로 암호화해서 주고 받음

#### Network 통신 3단계
- 단계1: Handshake
    - CA 공개키를 이용해서, 서버의 인증서 복호화
    - 서버측 공개키 Get
        - SSL에서는 공개키를 사용하지 않음
    - 과정
        - 클라이언트 서버 접속 (Client Hello), 데이터 주고 받음
            - 클라이언트 측에서 생성한 랜덤 데이터 (Client Random, for pre master secret 키)
            - 클라이언트가 지원하는 암호화 방식
            - 세션 아이디 (from client to server)
                - 연결에 대한 식별자
                - 이미 SSL 핸드쉐이킹을 했을 경우 기존의 세션 ID 재활용
                - 처음 연결이면 아직 세션 아이디가 없다.
        - 서버가 응답 (Server Hello), 데이터 주고 받음
            - 서버 측에서 생성한 랜덤 데이터 (Server Random, for pre master secret 키)
            - 서버가 선택한 암호화 방식
            - 인증서
        - 클라이언트가 CA 확인 후, pre master secret 키를 서버에 전송
            - 클라이언트가 CA리스트를 확인
            - 내장된 CA 공개키로 인증서 복호화
            - 성공 시 CA의 개인키로 암호화된 문서임을 확인
            - 클라이언트가 서버 랜덤 데이터와 클라이언트 랜덤 데이터를 조합하여 `pre master secret 키` 생성
            - Session 단계에서 암호화 하기 위해 사용
            - pre master secret은 대칭 키이기 때문에 노출 되면 안됨
            - 서버의 공개키(인증서 안에 들어 있음)로 pre master secret 값을 암호화해서 서버로 전송
        - 서버가 클라이언트의 pre master secret을 비공개키로 복호화 -> master secret -> session key
            - 클라이언트, 서버 둘다 pre master secret 소유
            - 서버와 클라이언트가, 특정 과정을 거쳐 pre master secret 값을 master secret 값으로 만듬
            - master secret으로 session key를 생성
            - session key로 서로 데이터를 대칭키 방식으로 암호화 한후 주고 받음
        - 핸드쉐이크 종료를 서로에게 전달
- 단계2: Session (전송)
    - 실제로 서버와 클라이언트가 데이터를 주고 받는 단계
    - 서로 알고 있는 session key 값을 이용해서 대칭키 방식으로 암호화
- 단계3: 전송 종료
    - 서로 SSL 통신이 끝났음을 알려줌
    - 대칭키인 세션키를 폐기

# Source
- https://opentutorials.org/course/228/4894
- https://whitelka.tistory.com/3