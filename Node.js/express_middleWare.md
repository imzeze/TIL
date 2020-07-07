Express는 라우팅 및 미들웨어 웹 프레임워크로, Express application은 미들웨어 함수를 호출한다.   
현재의 미들웨어 함수가 요청-응답 주기를 종료하지 않는 경우에는 `next()`를 호출하여 다음 미들웨어 함수에 제어를 전달한다. 그렇지 않으면 해당 요청은 정지된 채로 방지된다.  
Express 애플리케이션은 다음 유형의 미들웨어를 사용할 수 있다.
    
    * 애플리케이션 레벨 미들웨어
    * 라우터 레벨 미들웨어
    * 오류처리 미들웨어
    * 기본제공 미들웨어
    * 써드파티 미들웨어 

<br/>  

## 애플리케이션 레벨 미들웨어  

애플리케이션 레벨 미들웨어는 `express()`를 이용해 인스턴스를 생성한다.  
`.use()` 및 `.HTTPMETHOD()`를 이용해 생성한 인스턴스에 라우터 핸들러를 만들고, 미들웨어를 바인드한다. 라우터 핸들러는 stack으로, 여러 미들웨어를 바인드할 수 있다.    

먼저 다음 예제는 마운트 경로가 없는 라우터 핸들러에 미들웨어 함수를 바인드한 것이다. 이 미들웨어 함수들은 앱이 요청을 받을 때마다 실행된다.       

```js
var app = express();    
app.use(func());        // 모든 유형의 Http Method 처리 
app.get(func());        // get Method 처리
app.post(func());       // post Method 처리
```
마운트 경로가 있는 라우터 핸들러를 만들기 위해서 첫번째 인자에 경로를 넘겨준다. 
```js
app.use('/', func()); 
```
<br/>  

`next()`는 미들웨어의 다음 스택 혹은 다음 라우터 핸들러를 호출할 때 쓰인다.   
다음 코드에서 첫번째 라우터 핸들러의 미들웨어는 모두 실행되지만, 첫번째 핸들러에서 요청-응답 주기를 종료시켰으므로 두번째 라우터 핸들러는 절대 호출되지 않는다.

```js
app.use('/', function(req, res, next) {
    next();
}, function(req, res, next) {

});

//호출되지 않음
app.use('/', function(req, res, next){

});
```
<br/>  

`next('route')`는 미들웨어 하위 스택을 호출하지 않고 다음 라우터 핸들러를 호출한다. 단 `app.HTTPMETHOD()`나 `route.HTTPMETHOD()`를 이용한 라우트 핸들러에만 작동한다. 

```js
app.get('/', function (req, res, next) {
  if (req.params.id == 0) next('route');
  else next();
}, function (req, res, next) {
  //id != 0일 경우 호출됨
  res.render('regular');
});

//id == 0일 경우 호출됨
app.get('/', function (req, res, next) {
  res.render('special');
});
``` 
<br/>  

## 라우터 레벨 미들웨어
라우터 레벨 미들웨어는 `express.Router()`를 이용해 인스턴스를 생성한다.   
사용방법은 애플리케이션 레벨 미들웨어와 동일하다. 

<br/>    

## 오류처리 미들웨어
오류처리 미들웨어는 `err, req, res, next` 인자를 넘겨 정의할 수 있다.   
`next` 오브젝트는 사용할 필요없지만, 시그니처 유지를 위해 포함된다. 
```js 
app.use(function(err, req, res, next){
    console.err(err.stack);
});
```

<br/>  

## 기본 제공 미들웨어  
img, css, js 파일과 같은 정적 파일 제공을 위해 `express.static`이 쓰인다.   
인자로 디렉토리의 위치를 보내, 디렉토리에 포함된 파일들을 제공할 수 있다. 
```js
// 경로 : stylesheets/style.css
app.use(express.static(path.join(__dirname, 'public'))); 
```
가상 경로로 표시하려면 다음과 같이 첫번째 인자에 마운트 경로를 지정하면 된다. 
```js
// 경로 : static/stylesheets/style.css
app.use('static', express.static(path.join(__dirname, 'public'))); 
```
> `__dirname`은 Node.js에서 제공하는 키워드로, 현재 파일의 경로를 반환한다.   
파일명은 `__filename` 키워드를 이용해 얻을 수 있다.

<br/>  

## 써드파티 미들웨어
Express 앱에 기능을 추가하기 위해 사용된다.   
필요한 모듈을 설치한 후, 애플리케이션 레벨 미들웨어나 라우터 레벨 미들웨어에 모듈을 로드한다. 

```js
var app = express();
var cookieParser = require('cookie-parser');

app.use(cookieParser()); 
```