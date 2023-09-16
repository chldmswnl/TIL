# loading.js

### loading.js를 page.js와 같은 폴더 아래에 위치 시키면 Suspense를 사용한것 처럼 data fetching할때 로딩 상태를 표현할 수 있다.

```
--- dashboard
  --- page.js
  --- loading.js
```

#### Server component가 default이지만 Client component에서도 사용가능하다.

```
export default function Loading() {
  // Or a custom loading skeleton component
  return <p>'Loading...'</p>
}
```

#### 하지만 컴포넌트 전체를 감싸기 때문에 컴포넌트 안에 있는 컴포넌트 각각마다 적용시키고 싶다면 Suspense를 사용하는게 낫다.
