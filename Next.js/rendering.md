# Next.js의 4가지 렌더링 방식

### Next.js를 왜써? 하면 SSR때문에 쓰는거라고 말했던 무지했던 시절을 반성하며..

### Next.js에서는 4가지 렌더링방식을 지원한다.

- SSG (Static Site Generation)
- ISR (Incremental Static Regeneration)
- SSR (Server Side Rendering)
- CSR (Client Side Rendering)

Next.js 13 app router부터는 pages router와는 달리 컴포넌트별로 렌더링방식을 지정할 수 있는데 default는 SSG이다.

### SSG

렌더링하는 주체 = 서버

**장점**

- TTV(Time to view)가 빠름
- 자바스크립트가 필요 없음 (번들사이즈 down)
- SEO 최적화에 좋음 (HTML이 이미 생성)
- 보안이 뛰어남
- CDN 캐시가 됨

**단점**

- 빌드할때 렌더링 되기 때문에 데이터가 정적임
- 사용자별로 정보제공이 어려움

### ISR

렌더링하는 주체 = 서버

**장점**

- SSG 장점 전부 + 데이터가 주기적으로 변경

**단점**

- 실시간 데이터가 아님

```
// 3600초마다 데이터 갱신
fetch("https://...", {next: {revalidate: 3600 }})

or

export const revalidate=3600;
```

### SSR

렌더링하는 주체 = 서버

**장점**

- SSG 장점 전부 + 데이터가 실시간

**단점**

- 서버 과부하 (오버헤드)

```

fetch("https://...", { cache: 'no-store'})

```

### CSR

렌더링하는 주체 = 클라이언트

**장점**

- 로딩 완료시 빠른 UX제공
- 서버 부하가 적음

**단점**

- TTV(Time to view)가 길다
- 자바스크립트 활성화 필수
- SEO 최적화가 힘듬
- 보안에 취약함
- CDN에 캐시가 안됨

```
'use client' // 컴포넌트 상단에 선언 필수
```

### Next.js는 SSR이던 CSR이던 우선 빌드 될때 컴포넌트들을 돌아다니면서 HTML을 생성한다. CSR인 경우 코드(JS)를 클라이언트에 보내주고 Hydration이 일어난다.
