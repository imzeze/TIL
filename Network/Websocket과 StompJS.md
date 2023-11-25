# WebSocket

우선 HTTP 통신은 단방향 통신으로, 양방향 통신처럼 요청과 응답을 처리하기 위해 `Short Polling`, `Long Polling`과 같은 방법이 쓰인다. [(참고) Polling](https://github.com/imzeze/TIL/blob/master/Network/comet.md)

이와 달리 WebSocket은 양방향으로 통신할 수 있는 방법으로, 아래와 같은 특징을 갖는다.

1. WebSocket은 HTML5에서 등장한 통신 방법이다.  
   대부분의 브라우저에서 지원되나 IE와 같은 하위 브라우저에서 사용하지 못한다. ( 하위 브라우저를 지원하기 위한 모듈로 socket.io, Sock JS 등이 있다.)
2. HTTP 연결 후 WS 프로토콜로 전환되는 핸드쉐이킹 과정을 거친다.
   <img width="586" alt="image" src="https://github.com/imzeze/TIL/assets/67260437/0fe519ee-3b7e-47f1-94b9-414a682e84fa">

   > HTTP 프로토콜과 호환된다는 장점이 있다.
   >
   > - HTTP 프록시 지원
   > - 80번과 443번 포트 위에서 동작

3. 커넥션이 연결되면 서버와 클라이언트는 `메시지`를 보낼 수 있다.

4. 연결을 끊고자 하는 쪽에서 `프레임`을 보내 연결을 끊을 수 있다. 연결을 끊는 과정 역시 핸드쉐이킹 과정을 거친다.

<br />
<br />

# StompJS

Stomp JS는 WebSocket 위에서 동작하며, Stomp 프로토콜을 지원한다. 따라서 어떤 언어나 플랫폼, 메시지 브로커에서도 메시지를 쉽게 처리할 수 있다.  
StompJS는 주로 Spring에서 쓰이며 Node 서버에서는 주로 Socket.io가 사용된다.

> Stomp 프로토콜  
> (Streaming Text Oriented Messaging Protocol)  
> 서버/클라이언트 간에 주고 받는 메시지의 포맷을 정의한 규칙

> Message Broker  
> Publisher로부터 전달받은 메시지를 Subscriber에게 전달해주는 중간 역할을 한다.  
> ex) ActiveMQ

<br/>

## 주요 API

`Client`  
Stomp js의 entry point

`activate()`  
broker와 연결을 시작한다. 연결이 끊길 경우 재연결을 시도한다.

`deactivate()`  
연결된 경우 연결을 끊고 재연결 시도를 보내지 않는다.

`subscribe()`  
STOMP 브로커를 구독해 메시지를 받는다.

`unsubscribe()`  
구독을 취소한다.

`onWebSocketError()` / `onWebSocketClose()`
WebSocke에서 error가 발생하거나 WebSocket이 연결이 끊겼을 때 callback을 실행할 수 있다.

<br />

---

### reference

[stomp-js.github.io](https://stomp-js.github.io/)
