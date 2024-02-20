# Custom Document

```js
import { Html, Head, Main, NextScript } from "next/document";

export default function Document() {
  return (
    <Html lang="en">
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```

`_document` 파일은 `<html>`와 `<body>`를 수정할 수 있다. 이 컴포넌트는 서버에서만 렌더링되기 때문에 브라우저 이벤트를 받을 수 없다.

`<Html>`, `<Head />`, `<Main />`, `<NextScript />`는 반드시 포함돼야한다.

- `<Head />`는 `next/head`와 다르다.  
   `<Head />`는 모든 페이지에 `<head>`를 지정하는 데 쓰인다.  
   `next/head`는 Page 컴포넌트나 다른 컴포넌트에서 `<title>`와 같은 태그들을 사용하는 데 권장된다.
- `<Main />` 밖의 컴포넌트는 브라우저에 그려지지 않는다.

<br />
<br />

# Custom APP

`_app` 파일은 아래 목적으로 생성한다.

- page 간 공통 layout 지정
- 페이지에 포함될 데이터 추가
- global css 추가
