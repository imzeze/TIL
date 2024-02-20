# Server Component

next v13 APP Router부터 React Server Component가 생겼으며, 기본적으로 Server Component가 사용된다. Server Component는 서버에서 렌더되고 선택에 따라 캐시된다.

서버 렌더링 전략에는 아래 세가지가 있다.

<br />

## Static Rendering

Static Rendering을 사용하면 빌드될 때나 refetch할 때 백그라운드에서 렌더된다. 렌더된 컴포넌트는 캐시된다.

사용자에 따라 개인화될 필요가 없는 컨텐츠나 빌드할 때 데이터를 이미 갖고 있는 경우에 적합하다.

<br />

## Dynamic Rendering

사용자의 요청에 따라 렌더된다. 따라서 개인화될 필요가 있는 컨텐츠 또는 search param이나 cookie와 같이 요청이 들어올 때 정보를 얻을 수 있는 경우(Dynamic Functions)에 적합하다.

Dynamic Render된 페이지더라도 캐시된 데이터와 캐시되지 않은 데이터를 가질 수 있다. Next는 Dynamic Functions이나 캐시되지 않은 데이터를 발견하면 알아서 Danamic Rendering을 실행한다.

<br />

## Streaming

모든 데이터가 렌더되기 전에 상호작용할 수 있는 컴포넌트를 렌더할 수 있다.

<br/>

## 서버 렌더링 장점

- Data fetch  
  데이터 fetch를 서버에서 함으로써 요청 횟수를 줄일 수 있다.
- 캐시
- Bundle Sizes 경량화
- FCP 성능 개선
- SEO 및 SNS 공유
- Streaming  
  렌더링을 chunk와 stream으로 분리해 클라이언트는 서버에서 전체 페이지가 렌더되기까지 기다리지 않고 페이지 일부를 좀 더 일찍 볼 수 있다.
- 보안  
  토큰이나 API key를 클라이언트에 노출할 필요가 없다.

<br/>
<br/>

# Client Component

Client Component는 클라이언트의 요청이 들어올 때 렌더되며, 이를 사용하기 위해선 `"use client"` 라인을 추가해야한다. 이를 추가한 컴포넌트에 import된 모듈들과 하위 컴포넌트들 역시 client bundle에 추가된다.

Client Component를 사용하면 사용자의 interaction을 받아 UI를 업데이트하는 등의 작업을 할 수 있고, WEB API를 사용할 수 있다.

<br/>
<br/>

---

#### Reference

[Server component](https://nextjs.org/docs/app/building-your-application/rendering/server-components)

[Client component](https://nextjs.org/docs/app/building-your-application/rendering/client-components)
