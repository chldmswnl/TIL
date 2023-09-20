# Middleware

### Middleware란 request가 끝나기 전에 코드를 실행할 수 있게 해주는 것으로 쉽게 이야기하면 요청이 왔을때 중간에 낚아채 내가 의도한 행동을 할수 있게 한다. (일종의 문지기 역할)

#### Next.js에서 middleware를 사용하기 위해서는 pages, app 과 같은 레벨에 middleware.ts를 위치해놓거나 src폴더 안에 위치해놓으면 된다.

```tsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL("/home", request.url)); // 요청이 들어왔을때 '/home' 으로 redirect 시킨다.
}

export const config = {
  matcher: "/about/:path*", // matcher를 사용해서 진입점을 지정할 수 있는데 여기서는 '/about/아무거나'로 지정해 놓았다.
};
```

##### 즉 이 코드 블록이 의미하는 바는 '/about/me' 와 같은 경로로 접근했을때 '/home'으로 redirect 시켜주는 것이다.

##### middleware는 모든 루트에서 실행된다. Next.js에서는 아래 이미지 순서에 따라 실행된다고 설명한다.

<img width="590" alt="스크린샷 2023-09-19 오후 5 22 16" src="https://github.com/chldmswnl/TIL/assets/63483751/884413aa-53f2-4aa6-9189-9196f7bcc30f">

##### 모든 루트에서 실행되는 것이 싫다면 2가지 방법을 이용하여 middleware를 실행 할 수 있다.

#### 1. Matcher

##### 앞서 살펴본것처럼 config객체안에 matcher를 이용하여 path를 지정할 수 있다. 타입은 문자열 혹은 배열타입으로 하나 혹은 여러개의 path를 지정할 수 있다.

```tsx
export const config = {
  matcher: [
    /*
     * Match all request paths except for the ones starting with:
     * - api (API routes)
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     */
    "/((?!api|_next/static|_next/image|favicon.ico).*)",
  ],
};
```

##### 이렇게 정규식을 통해 path를 지정할 수 있다.

#### 2. Conditional Statements

```tsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith("/about")) {
    // pathname이 '/about'으로 시작 될때 조건문 시작
    return NextResponse.rewrite(new URL("/about-2", request.url));
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

request -> Cookie header \
response -> Set-Cookie

- request에서 오는 cookies에서 사용 할 수 있는 메서드: get, getAll, set, delete, has, clear
- response로 보낼 cookies에서 사용 할 수 있는 메세드: get, getAll, set, delete

```tsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  let cookie = request.cookies.get("nextjs");
  console.log(cookie); // => { name: 'nextjs', value: 'fast', Path: '/' }
  const allCookies = request.cookies.getAll();
  console.log(allCookies); // => [{ name: 'nextjs', value: 'fast' }]

  request.cookies.has("nextjs"); // => true
  request.cookies.delete("nextjs");
  request.cookies.has("nextjs"); // => false

  const response = NextResponse.next();
  response.cookies.set("vercel", "fast");
  response.cookies.set({
    name: "vercel",
    value: "fast",
    path: "/",
  });
  cookie = response.cookies.get("vercel");
  console.log(cookie); // => { name: 'vercel', value: 'fast', Path: '/' }
  // The outgoing response will have a `Set-Cookie:vercel=fast;path=/test` header.

  return response;
}
```

#### Setting Headers

request와 response header를 NextResponse API와 NextRequest API를 이용해서 설정할 수 있다.

```tsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  // Clone the request headers and set a new header `x-hello-from-middleware1`
  const requestHeaders = new Headers(request.headers);
  requestHeaders.set("x-hello-from-middleware1", "hello");

  // You can also set request headers in NextResponse.rewrite
  const response = NextResponse.next({
    request: {
      // New request headers
      headers: requestHeaders,
    },
  });

  // Set a new response header `x-hello-from-middleware2`
  response.headers.set("x-hello-from-middleware2", "hello");
  return response;
}
```

#### Producing a Response

Response와 NextResponse 인스턴스를 이용해서 Middleware에서 바로 응답할 수 있도록 할 수 있다.

```tsx
import { NextRequest, NextResponse } from "next/server";
import { isAuthenticated } from "@lib/auth";

// 만약 인증 되지 않은 요청이라면 401상태코드의 응답을 보낸다.
export function middleware(request: NextRequest) {
  if (!isAuthenticated(request)) {
    // Respond with JSON indicating an error message
    return new NextResponse(
      JSON.stringify({ success: false, message: "authentication failed" }),
      { status: 401, headers: { "content-type": "application/json" } }
    );
  }
}
```
