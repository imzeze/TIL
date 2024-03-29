# Polling

## HTTP 통신 방식

HTTP는 클라이언트의 요청이 있을 때만 서버가 응답하여 처리한 후에 연결을 끊는 **단방향 통신**이다.  
따라서 서버에서 클라이언트로 데이터를 전송하는 **Server Push** 가 불가능하다.  
하지만 실시간으로 변하는 데이터를 클라이언트에게 전달하기 위해 양방향 통신의 필요성이 대두되었다.  
이에 클라이언트가 양방향 통신이 되고 있는 것처럼 느낄 수 있도록 하는 일종의 편법이 고안되었다.  
즉, 실시간으로 작동하는 것이 아니라 짧은 시간 간격으로 웹페이지를 업데이트 하는 방법이다.  
여기에는 `Polling`, `Long Polling`, `Streaming` 등이 있다.

<br/>

## Polling

![polling](https://user-images.githubusercontent.com/67260437/102706281-ca083400-42d3-11eb-99fc-6609895537d1.png)

Polling은 클라이언트가 서버에 **주기적으로** 요청을 보내는 방식으로, 다음과 같은 특징을 갖는다.

- 시간마다 요청이 일괄적으로 전달된다.
- 실시간 통신이 중요한 서비스에 적합하지 않다.  
  실시간으로 통신하는 느낌을 주기 위해 요청 시간의 간격을 줄일 수 있으나 이는 서버에 무거운 부하를 주게된다.  
  이유는 다음의 프로세스를 반복해야 하기 때문이다.

      ```
      요청 발생 -> 연결 생성 -> HTTP header 파싱 -> 쿼리 수행 -> 응답 생성 -> 응답 전송 -> 연결 닫음
      ```

- 또한, 서버에서 보낼 데이터가 없어도 데이터를 전송해야하므로 서버의 리소스를 낭비하게 된다.

스포츠 경기를 시청하던 중 네이버의 실시간 중계 서비스를 확인하면, 경기 현황이 실시간으로 반영되지 않는 순간을 본 적이 있을 것이다. 바로 이와 같은 서비스를 Polling 방식을 따르는 서비스라고 할 수 있다.

<br/>

## Long Polling

![LongPolling](https://user-images.githubusercontent.com/67260437/102706298-e99f5c80-42d3-11eb-83fa-53eb01979709.png)

Long Polling은 위에서 언급한 Polling의 문제점을 해결하기 위한 방식으로, 클라이언트로부터 받은 요청을 유지하다가 데이터가 발생한 후(즉, 서버 이벤트가 발생되는 시점)에 응답을 전송하는 방식이다.  
단, 요청을 무한히 유지하는 것이 아니라 time out이 발생하면 해당 요청/응답 트랜잭션을 완료하고 연결을 새로 생성한다.  
Long Polling의 특징은 다음과 같다.

- 항상 연결이 유지되어 있기 때문에 사실상 실시간 통신이 가능하다.
- 데이터 이동이 빈번하지 않은 서비스에 적합하다.  
  Long Polling 방식의 채팅방에 1000명이 참여하고 있다고 가정해보자. 한명이 메시지를 연달아 계속해서 보낸다면, 1000명의 Request Queue가 순간적으로 쌓이게 되며 서버에 부하가 생길 수 있다.  
  또한 HTTP는 header가 무거운 프로토콜이기 때문에 요청의 증가에는 많은 비용이 따른다.

<br/>

## Streaming

![streaming](https://user-images.githubusercontent.com/67260437/102706303-fae86900-42d3-11eb-9528-d9e86b375bed.png)

Streaming 방식은 연결을 계속 유지하고, 이벤트가 발생할 때마다 부분적인 응답으로 브라우저에 데이터를 보내는 방식이다.  
응답마다 다시 요청해야 하는 Long Polling에 비해 효율적이며, 서버의 상태 변경이 매우 잦은 경우 유리하다.

<br/>

#### Reference

[Long Polling - Concepts and Considerations](https://www.ably.io/concepts/long-polling)  
[RTCS 실시간 웹 서비스를 위한 도전](https://d2.naver.com/helloworld/1052)  
[웹채팅 구현시 Long-Polling 방식과 Polling방식 선택하기](https://kuimoani.tistory.com/entry/%EC%9B%B9%EC%B1%84%ED%8C%85-%EA%B5%AC%ED%98%84%EC%8B%9C-Long-Polling-%EB%B0%A9%EC%8B%9D%EA%B3%BC-Polling%EB%B0%A9%EC%8B%9D-%EC%84%A0%ED%83%9D%ED%95%98%EA%B8%B0)
