## Server Side Rendering

### getServerSideProps

```js
const getServerSideProps:GetServerSideProps<RetrunType> = (context) => {
    return {props: result as returnType}
}
```

요청이 들어오면 데이터를 fetch하고 render한다.

**사용 예**

- 사용자별 데이터가 있는 페이지
- 요청 데이터에 영향을 받는 페이지 ex) auth, 위치 등

**특징**

- 서버에서 실행된다.
- JSON이 반환되고, 이 값은 초기 HTML 렌더에 쓰인다
- 페이지가 받는 props는 클라이언트 단에 노출되기 때문에 민감정보가 들어가 있지말아야 한다.

<br /><br />

## Static Generation

### getStateProps

```js
const getStaticProps:GetStaticProps<RetrunType> = (context) => {
    return {props: result as returnType}
}
```

함수를 export하면 Next는 이 페이지를 빌드할 때 미리 render한다.  
함수 호출 결과로 반환된 값은 `Page` 컴포넌트의 props로 받아 사용할 수 있다.

**사용 예**

- 빌드할 때 데이터를 이미 알고 있는 경우
- headless CMS를 사용하는 경우
- SEO가 필요하거나 캐시되어 빠르게 render 돼야하는 경우

**특징**

- 서버 측 코드를 직접 작성할 수 있다.
- 서버 사이드에서만 실행되기 때문에 클라이언트, 즉 browser에서 실행되지 않는다.
- HTML과 JSON이 모두 생성되고 JSON은 클라이언트 사이드에서 Props를 읽을 때 사용된다.
- 페이지에서만 export될 수 있다.
- 페이지가 받는 props는 클라이언트 단에 노출되기 때문에 민감정보가 들어가 있지말아야 한다.

<br/>

> [참고: headless CMS란]
>
> 웹사이트를 만들 때 필요한 데이터를 백엔드 필요없이 관리하고 프론트 스택에 상관없이 다양한 포맷으로 만들 수 있는 방법이다.  
> 컨텐츠 편집자는 GUI로 데이터를 편집하고 프론트엔드 개발자는 Headle CMS의 API로 데이터를 가져올 수 있다.
>
> 예: https://strapi.io/

<br />

### getStaticPath

Dynamic route에서 `getStaticProps`를 사용하는 경우 정적으로 생성되어야하는 페이지를 정의한다. `getStaticPath`에서 return된 값은 `getStaticProps`의 `context`에서 받을 수 있다.

```js
const getStaticPath: GetStaticPaths = () => {
  return {
    paths: [{ params: { key: value } }],
    fallback: false,
  };
};

const getStaticProps: GetStaticProps<RetrunType> = (context) => {
  const id = context.params.id;
};
```

**GetStaticPaths**

- paths
  - 정적으로 생성할 페이지 지정
  - dynamic router에 쓰인 파라미터를 키값으로 갖는다.
    ```
    paths: [{params: {id: 1}}]
    ```
- fallback
  - `false`  
     return되지 않은 페이지들은 404로 넘어간다.
  - `blocking`  
     첫 요청에 서버에서 render링하고(SSR), 생성된 HTML은 prerender 목록에 추가되어 이후 들어오는 요청에는 만들어진 페이지가 리턴된다.
  - `true`  
     먼저 호출된 페이지의 fallback을 보여준다. ex) loading  
     background에서는 HTML과 JSON이 생성되며 `getStaticProps`가 실행돼 페이지가 그려진다. 사용자는 fallback page에서 완료 페이지로 넘어간다고 느끼게 된다. 이 역시 prerender에 추가되어 이후 들어오는 요청에는 만들어진 페이지가 리턴된다.

<br /><br />

## Incremental Static Regeneration

페이지별로 정적 생성을 할 수 있다.

```js
const getStaticProps:GetStaticProps<RetrunType> = (context) => {
    return {
        props: result as returnType, revalidate: 10,
    }
}
```

- 최초 요청 후 10초 동안 들어온 요청에는 캐시된 페이지가 반환된다.
- 10초가 지난 후 들어온 요청에도 역시 캐시된 페이지가 반환되는데, 이때 background에서 페이지가 재생성된다.
- 재생성이 완료되면 캐시된 페이지는 없어지고, 재생성된 페이지가 반환되며, 만약 실패하면 기존에 캐시된 페이지를 보여준다.

**On-Demand Revalidation**  
v12.2.0 부터 특정 페이지를 재생성하는 On-Demand Revalidation이 가능해졌다.

먼저 revalidation API Route에 접근하기 위한 키를 생성한다.

```
https://<your-site.com>/api/revalidate?secret=<token>
```

```js
export default async function handler(req, res) {
  // secret 키가 일치하지 않는 경우
  if (req.query.secret !== process.env.MY_SECRET_TOKEN) {
    return res.status(401).json({ message: "Invalid token" });
  }

  try {
    // revalidate될 path 지정(실제 path여야함 )
    await res.revalidate("/path-to-revalidate");
    return res.json({ revalidated: true });
  } catch (err) {
    return res.status(500).send("Error revalidating");
  }
}
```

<br /><br />

## Client-side Fetching

**사용 예**

- SEO가 필요하지 않은 경우
- 데이터를 prerender할 필요가 없는 경우
- 업데이트가 자주 일어나야하는 경우

**특징**

- 컴포넌트 레벨 업데이트가 가능하다.

**SWR**  
클라이언트 단에서 데이터를 패치할 때 Next에서 추천하는 방식  
캐시, 재생성, refetch등을 관리한다.

<br /><br />

---

#### Reference

[Data Fetching](https://nextjs.org/docs/pages/building-your-application/data-fetching)
