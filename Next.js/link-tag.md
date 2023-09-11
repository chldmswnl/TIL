# Link

### Link 태그란 HTML의 a태그를 확장시킨 리액트 컴포넌트이고 client-side에서 pre-fetching 기능을 제공한다.

```
import Link from 'next/link'

export default function Page() {
  return <Link href="/dashboard">
            Dashboard
        </Link>
}

```

## Props

- **href (required)** : 가고자 하는 path or URL로 이동시켜줌. 객체를 같이 넘김으로서 query params 구현 가능

```
 <Link href="/dashboard" />

 // to /about?name=test

 <Link
  href={{
    pathname: '/about',
    query: { name: 'test' },
  }}
>
  About
</Link>
```

- replace : 브라우저 history스택에 새로운 URL을 추가하는 대신에 현재 history 상태로 교체. (default: false)

- scroll : Link태그의 기본동작은 새로운 페이지 이동시 스크롤을 페이지 최상단 혹은 그대로 유지시킨다. 하지만 false로 설정시 스크롤은 페이지 이동시 최상단에 이동하지 않는다. (default: true)

- prefetch : client-side navigation에서 성능을 최적화하기 위한 props로 viewport에 위치한 link태그로 이동될 수 있는 페이지들을 모두 pre-fetching 해놓는다. production모드에서만 동작한다. (default: true)

[참고 (Next.js docs)](https://nextjs.org/docs/app/api-reference/components/link)
