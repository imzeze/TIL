> [Sign in with Apple JS](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js) 번역글입니다.

> Expo로 개발 중인 사이드 프로젝트에서 애플 로그인을 구현한 방법에 대해서 정리하였습니다.
> Expo에서 [Expo AppleAuthentication](https://docs.expo.dev/versions/latest/sdk/apple-authentication/)을 지원하고 있으나 현재 안드로이드 기기에서 불가하며, 추후에 타 소셜 로그인도 추가할 예정이기 때문에 웹뷰로 구현하였습니다.

## Apple 로그인을 위한 웹페이지 구성

1. Authorization 객체와 로그인 버튼 생성

Javascript API를 사용해 Authorization 객체를 구성하였습니다.

```html
<html>
  <head> </head>
  <body>
    <script
      type="text/javascript"
      src="https://appleid.cdn-apple.com/appleauth/static/jsapi/appleid/1/en_US/appleid.auth.js"
    ></script>
    <div
      id="appleid-signin"
      data-color="black"
      data-border="true"
      data-type="sign in"
    ></div>
    <script type="text/javascript">
      AppleID.auth.init({
        clientId: "[CLIENT_ID]",
        scope: "[SCOPES]",
        redirectURI: "[REDIRECT_URI]",
        state: "[STATE]",
        nonce: "[NONCE]",
        usePopup: true,
      });
    </script>
  </body>
</html>
```

- clientId : Service ID의 Identifier 값
- scope : "name email"
- redirectURI : authorization code를 받기 위한 uri
- state : init function에서 전달된 상태
- nonce : request 재사용 방지를 위한 일회용값
- usePopup : true ? 팝업 노출 : 리다이렉트

2.
