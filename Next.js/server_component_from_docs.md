__이 파일은 공식문서를 보고 정리하였습니다.__

## Server component 

### Server component의 3가지 렌더링 전략 

- Static Rendering
- Dynamic Rendering
- Streaming

### Server rendering의 이점 
- Date fetching : 데이터 검색을 서버로 이동시켜 렌더링에 필요한 데이터를 가져오는 시간을 감소시킬수 있다. 
- Security : 노출되면 안되는 민감한 정보들을 안전하게 다룰수 있다. 
- Caching : 결과가 캐싱되어 지속되는 요청이나 다른 유저들에게 결과를 재사용할 수 있다. 
- Bundle size : 자바스크립트 번들 사이즈를 줄여 느린 인터넷이나 성능이 낮은 디바이스를 사용하는 유저들에게 이점이 있다.
- Initial Page Load and First Contentful Paint (FCP) : 서버에서 렌더링해서 보내줌으로서 최초 페이지 로드시간과 FCP지수를 향상시킬 수 있다.
- Search Engine Optimization and Social Network Shareability : SEO 향상
- Streaming: 렌더링 작업을 청크로 분할하고 준비가 될때마다 클라이언트로 스트리밍할 수 있다.

### Next.js에서는 서버컴포넌트를 기본적으로 사용한다. 그렇기 때문에 client component를 사용하기 위해서는 추가적인 설정이 필요하다.

### Server Rendering Strategies

#### Static Rendering (Default) 

Static rendering일때는 라우트가 빌드 타임에 렌더링된다. 따라서 결과들은 캐싱이 될 수 있고 CDN에 보관이 가능하다. (최적화) 

#### Dynamic Rendering 

Dynamic rendering일때는 요청때마다 렌더링이 된다. 이 렌더링 기법은 Static rendering기법과는 다르게 데이터가 사용자 맞춤 데이터이거나 요청타임에만 알 수 있는 정보(cookies, URL's search params)가 있을때 유용하다. 

##### Switching to Dynamic Rendering 

렌더링이 되는중에 dynamic function 혹은 uncached data request가 발견될 경우 Next.js는 dynamically rendering으로 변경한다. 

<img width="676" alt="스크린샷 2023-09-15 오전 10 07 46" src="https://github.com/chldmswnl/TIL/assets/63483751/ce5d1829-20ee-46e8-ac1a-ed2026f55623">

dynamic function은 밑에서 설명이 나오고 uncached data request의 경우 유저가 의도적으로 계속해서 데이터를 받아오기 위해 설정한 것들을 의미한다. 

##### Dynamic functions 

dynamic functions들은 요청할때마다 알 수 있는 정보들에 의존한다. 
- cookies(), headers()를 서버 컴포넌트에서 사용할 경우 전체 루트를 dynamic rendering으로 변경한다.
- useSearchParams()
- searchParams : pages에서 props로 사용시 페이지를 요청시간마다 dynamic rendering을 한다.

#### Streaming 

Streaming 사용시 라우트는 서버에서 요청때마다 렌더링된다. 각 진행은 청크 단위로 나뉘어 지고 클라이언트에서 준비됐을때 스트리밍된다. 이 기법은 유저로 하여금 전부 렌더링 되기전에
페이지의 일부를 보여줄 수 있게한다. 

Streaming은 낮은 우선순위의 UI나 느린 데이터 페칭으로 모든 페이지의 렌더링을 막는 경우에 유용하게 사용될 수 있다. 
