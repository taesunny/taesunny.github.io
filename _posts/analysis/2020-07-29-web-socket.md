---
title:  "WebSocket(웹소켓) 정리"
excerpt: "About WebSocket"

categories:
  - analysis
tags:
  - [websocket, analysis]

toc: true
toc_sticky: true
 
date: 2020-07-29
last_modified_at: 2020-07-29
---
# WebSocket

## 정의
단일 TCP connection으로 full-duplex(전이중) Communication 이 가능한 Communication Protocol
> - full-duplex communication : 전이중 통신, 하나의 전송 통로로 동시에 양쪽 방향으로 통신
> - half-duples communication : 반이중 통신, 하나의 전송 통로로 양쪽 방향 통신이 가능하나, 한번에 한쪽 방향으로만 통신이 가능하다.


## 특징
- HTTP 포트 80, 443(TLS 암호화 사용시)을 사용한다.
- Polling, Streaming과 같은 이전 방식들에 비해
- TCP handshake와 HTTP 헤더 부하에 의해 **작은 메시지** 전송시에는 비 효율적이다.
- HTML5 표준
- bytes 스트림 대신 UTF8 포멧의 메세지 스트림 이용


## 기존 방식
### Polling
client가 http request를 server로 요청을 지속적으로 날려서, event를 확인한다. http request 자체가 connection 부하가 있기 때문에, request를 계속 날리는 경우 server가 부담된다.

### Long Polling
client가 http request를 server로 요청을 날린 후, server에서 response를 event 후 날린다. event 발생 시 마다 request를 날리므로, event 발생 간격에 따라 비 효율적이다.

### Streaming
client가 http request를 server로 요청을 날린 후, 해당 요청을 끊지 않고, event 발생 시 response를 계속 전달한다. 기본적으로는 양방향 통신 불가


## Connection Flow
### handshaking
일반 HTTP request를 통해 handshaking

### data communications
server, client간 data frame 전송

### Closing
server, client간 close 요청 전송

## 기타 개념
### Socket.io
- node.js 기반
- 자체적으로 socket.io 서버를 만들어서, 클라이언트와 브라우저에 구애받지 않음. 
- Java 개발이 불가하다.

### SockJS
- Spring에서 websocket을 지원하기 위한 방식 중 하나
- WebSocket 미지원 브라우저 지원 가능

### STOMP
- Simple/Streaming Text Oriented Messaging Protocol의 약자
- 텍스트 기반의 메시징 프로토콜 
- TCP나 WebSocket과 같은 신뢰성있는 양방향 streaming network protocol상에 사용될 수 있다.

# Source
- https://en.wikipedia.org/wiki/WebSocket
- http://ghendra1610.blogspot.com/2012/05/how-does-asynchronous-web-work.html
- https://adrenal.tistory.com/20
- https://wondongho.tistory.com/73