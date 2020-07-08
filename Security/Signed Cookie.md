# Signed Cookie

Cookie는 Server와 Client 간에 전달되는 데이터로, 쉽게 노출되기 때문에 간단한 데이터여도 보안성을 갖추도록 서명이 필요하다.   
Express의 middleware `cookie-parser`를 이용해 다음과 같이 서명할 수 있다. 

```js
var express = require('express');
var cookieParser = require('cookie-parser');

var app = express();

app.use(cookieParser('123ABC!'));

app.use('/', (req, res) => {
  if(req.signedCookies.key) {
    res.send(`{key : ${req.signedCookies.key}}`)
  } else {
    res.cookie('key', 'value', {signed : true})
    res.end()
  }
});

module.exports = app;
```

쿠키는 서버에서 브라우저로 보내질 때 서명된다.

res 객체에 cookie를 생성할 때 `{signed : true}` 옵션만 주면 되는데,   

`cookieParser()`의 첫 번째 인자로 넣은 문자열로 쿠키가 서명된다. 

서명된 쿠키는 `req` 객체의 `signedCookies` 속성에 저장되므로 `req.signedCookies.쿠키명`과 같이 접근해야 한다. 

그 결과 네트워크 상에서는 다음과 같이 값이 암호화되고,   

![image](https://user-images.githubusercontent.com/67260437/86756225-33292180-c07d-11ea-84df-5a54e3af697d.png)  

웹 화면에는 값이 제대로 출력되는 것을 확인할 수 있다.
 

![image](https://user-images.githubusercontent.com/67260437/86756430-581d9480-c07d-11ea-8031-a855e6cfaf77.png)
