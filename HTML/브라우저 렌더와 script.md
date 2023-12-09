# script 태그 위치에 따른 차이점

브라우저의 렌더링 엔진은 아래 과정을 거쳐 html을 화면에 그린다.

<img width="625" alt="image" src="https://github.com/imzeze/TIL/assets/67260437/deaab322-1157-4f50-b83c-2ce4b74b7f58">

1. HTML 문서를 파싱하여 DOM 노드로 변환한다.
   DOM은 브라우저가 웹 페이지를 표현하는 방법이자, JS로 상호작용할 수 있는 데이터 구조이자 API이다.

2. 외부 CSS 파일 및 스타일 요소를 파싱해 렌더 트리를 구축한다.

3. 렌더 트리를 배치하고 화면에 그린다.

위 과정에서 `<script>`의 위치 및 속성이 렌더에 영향을 준다.

### 자바 스크립트가 파싱을 막을 수 있다

`<script>` 태그를 만나면 HTML 파서는 HTML 문서의 파싱을 일시 중지한 다음 JS 코드를 로딩하고 파싱해 실행해야 한다. JS는 DOM 구조 전체를 바꿀 수 있는 `document.write()` 메서드와 같은 것을 사용해 문서의 모양을 변경할 수 있기 때문이다.

이런 브라우저의 동작 방식은 두가지 중요한 이슈를 만든다.

- `<script>`에서는 `<script>` 아래에 있는 DOM 요소에 접근할 수 없다.
- 페이지 위쪽에 용량이 큰 `<script>`가 있는 경우 페이지에 접속하는 사용자들은 `<script>`를 다운받고 실행할 때까지 `<script>` 아래쪽 페이지를 볼 수 없다.

<br />

## `<head>` 태그와 `<body>` 태그 끝

HTML 파서가 `<head>`의 `<script>`를 만나면 파싱을 멈추고 스크립트를 처리한 후에 `<body>`를 파싱하기 때문에 이런 지연과정이 사용자 측면에서 바람직하지는 않다.

이에 대한 일반적인 해결방법은
DOM을 먼저 생성하도록 `<script>`를 페이지의 하단, 즉 `</body>` 직전에 삽입하는 것이다.

하지만 이 방법도 문서의 크기에 따라 완벽한 해결방법이 될 순 없다. 이런 문제를 해결할 수 있는 방법으로 HTML5의 `defer/async` 속성이 있다.

<br />

## defer / async 속성 사용

JS에서 `document.write()` 메서드를 사용하지 않는다면 `<script>` 태그에 `async` 속성이나 `defer` 속성을 추가할 수 있다.

### defer

```html
<script defer src=""></script>
```

- 브라우저는 defer 속성이 있는 스크립트를 백그라운드에서 다운로드한다.
- 페이지 생성을 절대 막지 않는다.
- DOM이 준비된 후에 실행되긴 하지만 DOMContentLoaded 이벤트 발생 전에 실행된다.
- defer 스크립트 역시 html에 추가된 순서대로 실행된다. 만약 스크립트가 병렬적으로 다운되는 상황에서 크기가 작은 스크립트가 먼저 다운로드가 끝나더라도 그 앞에 다운로드가 끝나지 않은 스크립트가 있다면 먼저 실행되지 않고 앞선 파일이 실행되기를 기다린다.

### async

```html
<script async src=""></script>
```

- 백그라운드에서 다운로드된다.
- DOMContentLoaded 이벤트와 async 스크립트 실행 간에 아무 관계가 없다.
- async 스크립트의 실행 순서는 다운로드 순이다. 이렇게 먼저 로드가 된 스크립트가 먼저 실행되는 것을 `load-first order`라고 한다.
- 다운로드가 모두 완료되면 HTML 파싱을 멈추고 스크립트를 실행한다.

<br />

---

#### Reference

[브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)

[최신 브라우저의 내부 살펴보기 3 - 렌더러 프로세스의 내부 동작](https://d2.naver.com/helloworld/5237120)

[defer, async 스크립트](https://ko.javascript.info/script-async-defer)

[외부 JavaScript 불러오기 (async, defer)
](https://rottk.tistory.com/entry/%EC%99%B8%EB%B6%80-JavaScript-%EB%B6%88%EB%9F%AC%EC%98%A4%EA%B8%B0-async-defer#toc2)
