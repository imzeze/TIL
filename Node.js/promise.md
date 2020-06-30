# promise

promise는 비동기 처리에 사용되는 객체이다. 
> 비동기 처리란 특정 코드의 실행이 완료되기를 기다리지 않고 다음 코드를 먼저 실행하는 것을 말한다.
비동기로 데이터를 가져올 때, 아직 가져오지 않은 데이터를 화면에 뿌리려고 하면 에러가 나기 때문에 이를 위한 해결책 중 하나로 쓰이는 방법이다.   

프로미스는 생성되고 종료될 때까지 `Pending`, `Fulfilled`, `Rejected`의 상태를 갖는다.
> Pending : 비동기 처리 전 상태  
Fulfilled : 비동기 처리를 완료하고 결과 값을 반환한 상태  
Rejected : 비동기 처리가 실패 상태

```js
// promise 사용 예제
function getData() {
    return new Promise(function(resolve, reject) {
        $.get('url', function(response){
            if(response) resolve(response);
            reject(new Error("fail"));
        });
    });
}

getData().then(function(data) {
    console.log(data);
}).catch(function(err) {
    console.log(err);
});
```
`getData()`의 실행으로 `Promise`객체를 반환 받는다. 비동기 처리를 성공하면 `resolve()`가 실행되고, `then()`을 이용해 결과 값을 받을 수 있다. 만약 비동기 처리에 실패하면 `reject()`가 실행되며 `catch()`를 이용해 error를 잡을 수 있다.   
`then()` 메소드의 경우, 호출 후 새로운 promise객체를 반환하기 때문에 다음과 같이 각 promise를 연결할 수 있다. 

```js
new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(1);
  }, 2000);
}).then(function(data){
    console.log(data); // 1
    return data + 10;
}).then(function(result) {
    console.log(result); // 11
    return result + 20;
}).then(function(result) {
    console.log(result); // 31
});
```
<br/>  

----
#### Reference

[자바스크립트 Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)