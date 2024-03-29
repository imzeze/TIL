# Cookie와 Session

HTTP는 비연결성과 무상태성을 가진 모델이기 때문에, 서버는 클라이언트 정보를 식별하기 위해 Cookie, Session, JWT 등을 사용한다.

이러한 인증 방법이 없다면 클라이언트는 서버와 새로운 연결을 맺을 때마다 로그인을 해야하는 불편한 상황이 생긴다.

<br />

## Cookie

서버는 사용자를 식별하기 위한 값을 Response Header의 `Set-Cookie`로 보낼 수 있다. 여기에 담긴 값은 브라우저에 자동으로 저장되어, 다음 요청마다 Request Header의 `Cookie`에 담겨 보내진다.

사용자는 브라우저에서 Cookie를 확인할 수 있기 때문에 Cookie에 중요 정보를 그대로 저장하는 것은 위험하다.

쿠키는 기본적으로 브라우저가 종료될 때 (세션이 종료될 때) 삭제되지만, Expires나 Max-Age가 설정된 경우에는 만료시점이 될 때까지 남아 있다.

<br />

## Session

Cookie에 사용자의 중요 정보를 그대로 저장하는 것은 보안상 위험하기 때문에 서버는 중요 정보를 대신해 사용자를 식별할 수 있는 Session ID 발급하고, 위와 같이 `Set-Cookie`를 통해 쿠키에 저장한다.

서버는 이후에 들어오는 요청에서 Session ID를 확인하여 사용자 정보를 조회한다.

### Session 하이재킹

세션 ID를 사용한다고 해서, 보안상 문제점이 없는 것은 아니다. 세션 ID를 탈취해 로그인 상태를 가로챌 수 있기 때문이다.

이를 방지하기 위해서는, 아래와 같은 점을 고려해야한다.

- 무작위 추측 대입 공격을 대비해 세션 ID를 충분히 큰 값으로 생성한다.
- 일정 주기마다 세션 ID를 재생성하여 연결을 지속하거나, 장기간 접속하지 않은 사용자의 경우 재로그인을 유도한다.
- 로그아웃 시에 세션 ID를 파기한다.
