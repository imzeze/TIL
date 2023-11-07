# JavaScript modules

브라우저에서 Javascript의 역할이 커지면서 JavaScript 모듈화를 위한 시스템이 생겨났다. 여기에는 `Common JS`, `AMD`, `Webpack`이 있다.

> **Common JS**  
> Common JS에서 모듈화 아래 3가지 요소로 이루어진다.
>
> - scope: 모듈의 실행 영역
> - export: 모듈 정의
> - require: 모듈 사용

> **Webpack**  
> bundler  
> 모듈의 의존성을 파악해 html이 실행할 수 있는 하나의 자바스크립트 파일로 합쳐준다. html은 이 파일을 script로 불러와 실행한다.

브라우저 역시 모듈화를 지원한다.

[예제 - basic-module](https://mdn.github.io/js-examples/module-examples/basic-modules/)

위 예제에서 `<script type="module">` 태그로 `main.js` 파일을 읽고, `main.js`는 모듈을 import 해서 사용하는 것을 확인할 수 있다.

모듈화가 되었으므로 각각의 모듈 파일은 서로의 스코프를 공유하지 않는다.

---

#### Reference

[JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

[JavaScript 표준을 위한 움직임: CommonJS와 AMD](https://d2.naver.com/helloworld/12864)
