# Json Web Token 

* JSON 포맷을 이용한 Web Token
* **Claim** 기반 Token (JWT에 담긴 정보의 조각을 Claim 이라고 한다.)
* 토큰 자체가 정보인 **Self-contained** 방식
* **Stateless** 상태를 지향한다.   

### JWT와 Session

Session은 서버에 저장되어 클라이언트에서 요청이 들어올 때마다 서버에 저장된 정보를 읽어온다. 클라이언트에 저장되는 Cookie 방식보다 안전하지만 모든 요청에 대해 서버를 조회해야 하기 때문에 부하가 생길 수 있고, 서버 확장 시에 세션 정보 동기화에 문제가 생길 수 있다.   

JWT는 토큰 자체로 정보를 가지고 있기 때문에 서버에 정보를 저장할 필요가 없다. 

<br/>  

## JWT Architecture

JWT의 기본 구조는 다음과 같다. 
```
Header . Payload . Signature 
```
### Header
```
{
  "typ": "JWT",
  "alg": "HS256"
}
```
`typ : JWT` 는 토큰 타입이 JWT임을 명시한다.   
`alg`는 해싱 알고리즘으로, 토큰을 검증 할 때 사용되는 signature에서 사용된다. 

### Payload
Payload에는 토큰에 담을 정보가 들어있다. 이 정보의 조각을 Claim이라고 하며 Claim은 `registered Claim`, `public Claim`, `private Claim`으로 나뉜다. 

**registered Claim**은 토큰에 대한 정보를 담는다.

| name | value | 
| --- | --- |
| iss | 토큰 발급자 |
| sub | 토큰 제목 | 
| aud | 토큰 대상자 | 
| exp | 토큰 만료시간 | 
| nbf | 토큰 활성 시각 ( Not Before ) |
| iat | 토큰이 발급된 시각 |  
| jti | JWT 고유 식별값 | 

**public claim**은 공개용 정보를 저장하는 Claim이다. 충돌방지를 위해 name을 url 형식으로 짓는다. 

**private claim**은 서버와 클라이언트 사이에 임의로 지정한 정보를 저장한다. public claim과 달리 충돌이 발생할 수 있다. 

### Signature

Signature은 Header와 Payload값이 변조되지 않았음을 보여주는 값이다. Header와 Payload의 값을 `base64`로 인코딩한 값을 Header에서 정의한 해싱 알고리즘을 이용해 암호화하고 다시 이 값을 `base64`로 인코딩하여 생성된다. 따라서 토큰 생성 시 만들어진 Signature값과 이후의 Signature값이 다르다면 Header나 Payload의 값이 변조되었다는 것을 의미한다. Header와 Payload는 외부에서 접근 가능하기 때문에 보호되어야 하는 데이터는 넣지 않도록 주의하고, 중요한 데이터는 JWE로 암호화한다. 

[JWT 암호화, 복호화 참고](https://jwt.io/)

<br/>

## 단점
 
* JWT는 상태를 저장하지 않기 때문에 한번 만들어지면 제어가 불가능하다. 즉, 토큰을 임의로 삭제하는 것이 불가능 하므로 토큰 만료 시간을 꼭 지정해줘야한다. 
* 정보가 많아질수록 토큰의 길이가 늘어나 네트워크에 부하를 줄 수 있다. 
* 중요한 데이터는 Payload에 넣지않고, JWE로 암호화해야한다.  





<br/> 

---
#### Reference

[[JWT/JSON Web Token] 로그인 / 인증에서 Token 사용하기](https://sanghaklee.tistory.com/47)  
[[Server] JWT(Json Web Token)란?](https://mangkyu.tistory.com/56)