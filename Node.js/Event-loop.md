# Event Loop 

## Call stack

Event Loop는 계속해서 call stack에 실행할 function이 있는지 살핀다. call stack이 비어있다면 실행되어야할 함수를 call stack에 담고, 하나씩 순서대로(LIFO) 실행한다.

```js
const bar = () => console.log('bar')
const baz = () => console.log('baz')
const foo = () => {
    console.log('foo')
    bar()
    baz()
}

func3()
```  
위 코드를 실행한 결과는 다음과 같다. 

```js 
foo
bar
baz
```

아래 call stack에 쌓이고 실행된 후 사라지는 것을 보자. 

![callStack](https://user-images.githubusercontent.com/67260437/85374494-d3f0ea80-b56f-11ea-8d32-0ced3586966b.png)

이번에는 `bar()` 함수 호출을 `setTimeout()`으로 지연시키는 예제이다. 
```js
const bar = () => console.log('bar')
const baz = () => console.log('baz')
const foo = () => {
    console.log('foo')
    setTimeout(bar(),0)
    baz()
}

func3()
```
0초 뒤에 `bar()`를 호출하도록 되어 있기 때문에 즉시 실행될 것 같지만, 결과는 다음과 같다.
```js
foo
baz
bar
```

![image](https://user-images.githubusercontent.com/67260437/85375286-09e29e80-b571-11ea-935a-8782ebe612b5.png)

`setTimeout()`이 call stack에 쌓이고 실행될 때 call back 함수 `bar()`는 백그라운드에 넘겨진다. 백그라운드에서는 `setTimeout()` 설정한 시간이 지난 뒤 **Message Queue**에 call back 함수 `bar()`를 보낸다. Event Loop는 앞서 말한대로 call stack이 빈 공간인지 계속해서 확인하다가, 빈 상태가 되었을 때 Message Queue에 남아 있는 함수들을 call stack에 쌓고, 실행시킨다. 때문에 0초로 설정했음에도 `baz()`가 먼저 실행된 것이다.   

만약 Task Queue에 쌓인 함수들이 많다면 어떨까? 쌓여있던 함수들이 실행된 후에야 `bar()`가 실행될 수 있을 것이다. 때문에 `setTimeout()`의 시간은 정확할 수 없다. 

<br/>  

### Task Queue와 Message Queue

[Difference between TaskQueue and MessageQueue](https://stackoverflow.com/questions/10075817/message-queue-vs-task-queue-difference) 

위에서 확인해 본 바 Message Queue는 Message를 단순히 전달 역할만 하는 Queue이고, Task Queue는 Task를 받아 처리한 결과를 반환하는 Queue인 것 같다. 

<br/>  

### 정리   

Evnet Loop는 Node.js의 가장 중요한 특징으로, 어떻게 Node.js가 비동기일 수 있는지, non-blocking I/O의 특징을 갖는지를 설명해준다.   

Node.js는 JavaScript code를 Single Thread로 처리한다.   
Single Thread로 처리된다는 것은 한번에 하나의 작업만 일어난다는 것을 의미한다. 이는 동시성의 문제를 해결하지만 개발자는 Thread가 에러로 인해 멈추지 않도록 유념해야한다. 

일반적으로 대부분의 부라우저는 모든 브라우저 탭마다 하나의 Event Loop를 갖는다. 모든 process가 독립적이도록 하고, 한 웹 페이지에서의 infinite loop가 다른 웹 페이지들에 영향을 끼치지 않도록 하기 위해서다.

---
#### Reference 
[공식 문서](https://nodejs.dev/learn/the-nodejs-event-loop) 
