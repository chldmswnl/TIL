# Middleware 

### Middleware란 request가 끝나기 전에 코드를 실행할 수 있게 해주는 것으로 쉽게 이야기하면 요청이 왔을때 중간에 낚아채 내가 의도한 행동을 할수 있게 한다. (일종의 문지기 역할)

#### Next.js에서 middleware를 사용하기 위해서는 pages, app 과 같은 레벨에 middleware.ts를 위치해놓거나 src폴더 안에 위치해놓으면 된다. 

```
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/home', request.url)) // 요청이 들어왔을때 '/home' 으로 redirect 시킨다. 
}

export const config = {
  matcher: '/about/:path*', // matcher를 사용해서 진입점을 지정할 수 있는데 여기서는 '/about/아무거나'로 지정해 놓았다. 
}
```

##### 즉 이 코드 블록이 의미하는 바는 '/about/me' 와 같은 경로로 접근했을때 '/home'으로 redirect 시켜주는 것이다. 

##### middleware는 모든 루트에서 실행된다. Next.js에서는 아래 이미지 순서에 따라 실행된다고 설명한다. 

<img width="590" alt="스크린샷 2023-09-19 오후 5 22 16" src="https://github.com/chldmswnl/TIL/assets/63483751/884413aa-53f2-4aa6-9189-9196f7bcc30f">

##### 모든 루트에서 실행되는 것이 싫다면 2가지 방법을 이용하여 middleware를 실행 할 수 있다. 

#### 1. Matcher

##### 앞서 살펴본것처럼 config객체안에 matcher를 이용하여 path를 지정할 수 있다. 타입은 문자열 혹은 배열타입으로 하나 혹은 여러개의 path를 지정할 수 있다. 

```
export const config = {
  matcher: [
    /*
     * Match all request paths except for the ones starting with:
     * - api (API routes)
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     */
    '/((?!api|_next/static|_next/image|favicon.ico).*)',
  ],
}
```

##### 이렇게 정규식을 통해 path를 지정할 수 있다. 

#### 2. Conditional Statements 


```
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith('/about')) { // pathname이 '/about'으로 시작 될때 조건문 시작 
    return NextResponse.rewrite(new URL('/about-2', request.url))
  }
}
```

#### NextResponse API 역할 

##### 앞서 살펴본 NextResponse는 아래와 같은 역할을 수행할 수 있다. 

- 들어오는 요청을 다른 URL로 redirect 시킬 수 있다.
- 주어진 URL로 rewrite 할 수 있다.
- API Routes, getServerSideProps 그리고 rewrite를 위해 request headers를 설정할 수 있다.
- cookies를 설정할 수 있다.
- headers를 설정할 수 있다.

#### Cookies
