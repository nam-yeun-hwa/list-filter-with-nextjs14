# 기술 스택
- next.js 14.0.4
- typescript

# Next.js 14 버전에서 달라진 점

Next.js 앱의 컴포넌트와 페이지에 대해 생각하는 방식에 큰 패러다임 변화를 가져왔습니다. 
Next.js 13 버전 이후부터는 Using App Router와 Using Pages Router을 제공 합니다.
기존버전의 경우는 Pages Router를 사용했다면 13버전 이후로 새롭게 App Router가 추가 되었습니다.

### App Router 뭔가요? </br>
App 디렉터리는 Next.js에서 라우트를 처리하고 뷰를 렌더링하기 위한 새로운 전략입니다. 

기존의 Next.js에서는 여러 파일에 나누어진 페이지를 생성하고 매핑하는 방식을 사용했습니다. 이로 인해 복잡성이 증가할 수 있었습니다. App Router는 페이지 구조를 더 단순화하고 일관성을 증가시킴으로써 개발자 경험을 향상시킬 수 있습니다.
App Router는 페이지 및 라우팅 로직을 통합하여 페이지 간 이동을 보다 효율적으로 관리할 수 있게 합니다. 이로써 라우팅 구성이 더욱 직관적이고 관리하기 쉬워집니다.</br></br>

### App Router 가장 크게 다른 점
  - 각종 폴더 유형 추가로 디렉토리 라우팅이 편해짐
  - 레이아웃 기능(layout.tsx 컴포넌트로 공통 레이아웃 적용)
  - 페이지별 권한 체크(next-auth@5)
  - 서버 컴포넌트 분리로 인한 최적화 </br>
    서버 컴포넌트를 설명 하자면 넥스트 서버에서 HTML을 미리 렌더링해서 프론트 또는 브라우저로 완성된 HTML를 미리 보내줍니다. </br>
    예전에 Pages Router를 사용했을때 각각의 페이지에서 캐싱 기능을 구현하였다면 App Router에서는 캐싱등의 기능을 넥스트 서버에서 대신하여 처리해 줍니다. </br>
    
  - 데이터 캐시
  - 서버 액션

# Next.js 14 버전에서 추가된 기능 설명

### Routing Group
  - 상태에 따른 폴더 이름으로 카테고리(레이아웃) 나누기용 
  - 실제 주소에는 관여하지 않는다.
  - 소괄호를 사용하여 폴더이름을 지정한다.

예) (beforelogin)
    (afterlogin)

공통 레이아웃이 다를경우 페이지를 나눠줄 수 있고 
beforelogin의 경우에 layout.tsx를 생성하여 레이아웃을 지정해 줄수도 있다.


### Parallel Route <br/>
  - page.tsx 두개를 동시에 보여주고 싶을때 사용한다.
  - 기존 페이지를 배경으로 두고 모달을 새루운 경로로 띄울때 사용한다. <br/>
  - 아래처럼 page.tsx와 @modal 루트경로가 같아야 한다. <br/>
  - 잘 살펴보면 같은 루트경로에서 @modal에서 모달로 띄우는 경로와 배경이 되는 페이지의 경로가 같다.

(-- 폴더 이미지 보여주도록 수정 --)  

**📄 layout.tsx**
- layout.tsx에 modal을 props로 받아주어야 한다.
- layout.tsx에서 page.tsx는 children으로 modal은 modal로 렌더링 된다.
  
  ```shell
  export default function layout({children, modal}){
    retun(
          <>
            {childrend}
            {modal}
          <>
        )
  }
  ```

**📄 default.tsx**

@madal폴더에도 page.tsx가 필요한데 넣을 내용이 없을 경우에는 default.tsx를 추가 해주어야 오류가 뜨지 않는다. </br>

```shell
export default function Default() {
  return null;
}
```

<img width="174" alt="스크린샷 2023-12-17 오후 3 11 22" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/05b87351-8b31-436f-aa4b-afe8458df18a">

### Interception Routes <br/>
  - 서로 주소가 다른 page.tsx를 띄울수 있게 해준다.
  - layout.tsx 에서 @modal이하 page.tsx는 @modal에서 뜨고 나머지 page.tsx는 children에서 뜨게 해준다. </br></br>
  방법은 parallel 라우트로 @modal 폴더를 두고 내부에 (..)i 로 폴더를 구성해주면 원래 주소인 /i/flow/login/page.tsx 가 @modal/(..)i/flow/login으로 interceptor 결국 i/flow/login/경로에는 @modal의 page.tsx 화면이 함께 화면에 보이게 된다. </br>
  i/flow/login/page.tsx는 화면대신 @modal/(..)i/flow/login/page.tsx가 화면에 보인다.</br>

 <img width="256" alt="스크린샷 2023-12-09 오후 10 39 19" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/ef7e0bb8-8d08-43cc-830b-d3041408f5d6">


따라서 parallel라우트와 intercepter 라우트를 함께 사용하면 페이를 배경으로 두고 새로운 페이지를 화면에 띄울 수 있다. </br>

- intercepter 되지 않은 상태의 i/flow/login/page.tsx는 interceptor된 상태의 페이지를 새로고침 하면 interceptor하지 않은 라우터로 렌더링 된다.
 <img width="253" alt="스크린샷 2023-12-09 오후 10 55 47" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/e3cdd867-99ce-42b9-ac1e-39da3647fc5d">


  (beforeLogin) 폴더의 page.tsx의 링크 이동을 눌러 페이지가 지동할 경우에는 interceptor된 @modal/(..)i/flow/login/page.tsx으로 뜨고 직접 /i/flow/login/ 주소를 입력하거나 interceptor된 상태에서 새로고침을 하면 interceptor되지 않은 원 /i/flow/login/page.tsx 페이지가 화면에 보인다.

### private 폴더 _component 
app폴더 하위이 _component폴더를 사용하여 내부에 컴포넌트를 만들면 실제 주소에는 관여하지 않고 공통되는 페이지를 화면에 보여 줄수 있다. 

### 서버 컴포넌트에서는 useState, useEffect등을 사용할 수 없다. 

  - page.tsx, layout.tsx 등은 모두 서버 컴포넌트이기 때문에 화면에 렌더링 된후에 사용하는 useState나 useEffect를 사용할 수 없다. <br/>
  - 이런 경우에는 서버 컴포넌트를 클라이언트 컴포넌트도 변경 해주어야 한다. <br/>
  - 클라이언트 컴포넌트로 변경하는 방법은 page.tsx나 layout.tsx 상단에 "use client"; 를 추가해 주면 된다. <br/>
  
# 여러가지 Hook

### useSelectedLayoutSegment
현재 나의 라우터 위치를 알아낼 수 있는 훅을 next에서 지원해 준다.
useSelectedLayoutSegment는 서브 컴포넌트가 아닌 클라이언트 컴포넌트에서 가능 하다.

```shell
const segment = useSelectedLayoutSegment();
console.log(segment) // 현재 활성화된 상위 라우터 주소

const segment = useSelectedLayoutSegments();
console.log(segment) // 현재 활성화된 상위, 하위 라우터 주소 ['compose', 'tweet'];
```

### usePathname

const pathname = usePathname() </br>
- pathname에 따른 렌더링 분기처리
  
```shell
"use client";
import {usePathname} from "next/navigation";

export default function RightSearchZone() {
  const pathname = usePathname()

  if (pathname === '/explore') {
    return null;
  }
  if (pathname === '/search') {
    return (
      <div></div>
    );
  }
  return (
    <div style={{ marginBottom: 60, width: 'inherit' }}>
      <SearchForm />
    </div>
  )
}
```

### useSearchParam
query string 받아오기

```shell
  'use client'

  const router = useRouter();
  const searchParams = useSearchParams();

  const onClickGotoPage = () => {
    //query string중 q값 가져오기
    router.replace(`/search?q=${searchParams.get('q')}`)
    //query string중 전체 값 가져오기
    router.replace(`/search?q=${searchParams.toString()}`)

  }
```

### useSession(next-auth@5)
로그인 후 내 정보를 useSession에서 불러 올 수 있다.

```shell
import {signIn} from "next-auth/react"; // 프론트엔드 서버 사용시

const { data: me } = useSession();

const onLogout = () => {
    signOut({ redirect: false }) //redirect를 true로 하면 서버에서 redirect를 실행 한다.
      .then(() => {
        router.replace('/');
      });
  };
```

다른 사용법 예제
```shell
import Main from "@/app/(beforeLogin)/_component/Main";
import {auth} from "@/auth";
import {redirect} from "next/navigation";

export default async function Home() {
  const session = await auth();
  if (session?.user) {
    redirect('/home');
    return null;
  }
  return (
    <Main />
  )
}

```

현재 세션 값을 이용하여 로그인 여부에 따라 화면에서 안보이게 보이게 컨트롤이 가능하다.

```shell
const session = await auth();
```



### useFormState와 useFormStatus 적용하기

# 그 외

### 다이나믹 라우팅 슬러그들의 value 값 받기

props에서 params속성안에 슬러그들의 값을 참조 할수 있다.

```shell
type Props = {
  params: { username: string, id: string, photoId: string }
}

export default function Page({ params }: Props) {
  params.username // elonmusk
  params.id // 1
  params.photoId // 1
  return (
    <Home />
  )
}
```

### new URLSearchParams 사용하기

URLSearchParams는 URL 쿼리 문자열을 다루는 데 유용한 인터페이스를 제공합니다.

```shell
    import { useRouter } from 'next/router';
    
    const MyPage = () => {
    
      const pathname = usePathname()
      const searchParams = useSearchParams();
      const router = useRouter();
    
      const onChangeFollow = () => {
        const newSearchParams = new URLSearchParams(searchParams);
        newSearchParams.set('pf', 'on');
        router.replace(`/search?${newSearchParams.toString()}`);
      }
      const onChangeAll = () => {
        const newSearchParams = new URLSearchParams(searchParams);
        newSearchParams.delete('pf');
        router.replace(`/search?${newSearchParams.toString()}`);
      }
    
      return (
        <div>
          ...
        </div>
      );
    };
    
    export default MyPage;

```

#### queryString 'pf=on' 값을 추가 할때

```shell
    const newSearchParams = new URLSearchParams(searchParams);
    newSearchParams.set('pf', 'on');
```
#### queryString 'pf' 값을 삭제 할때

```shell
    const newSearchParams = new URLSearchParams(searchParams);
    newSearchParams.delete('pf');
```

이렇게하면 주어진 URL의 쿼리 문자열을 다룰 수 있습니다.

### 클라이언트 컴포넌트와 서버컴포넌트로 구성하기

PostArtcle 컴포넌트에서 자식 프롭스로 children을 받아 화면에 표시해 주고 있다. </br>
PostArtcle 컴포넌트는 클라이언트 컴포넌트이다.

만약 서버 컴포넌트를 import하여 사용할 경우 서버컴포넌트가 클라이언트 컴포넌트 성격이 변경 된다.

- 부모컴포넌트
  
```shell
"use client";

import {ReactNode} from "react";
import style from './post.module.css';
import {useRouter} from "next/navigation";

type Props = {
  children: ReactNode,
  post: {
    postId: number;
    content: string,
    User: {
      id: string,
      nickname: string,
      image: string,
    },
    createdAt: Date,
    Images: any[],
  }
}

export default function PostArticle({ children, post}: Props) {
  const router = useRouter();
  const onClick = () => {
    router.push(`/${post.User.id}/status/${post.postId}`);
  }

  return (
    <article onClickCapture={onClick} className={style.post}>
      {children}
    </article>
  );
}
```

- 자식컴포넌트
  
```shell

type Props = {
  noImage?: boolean
}
export default function Post({ noImage }: Props) {
  const target = {
    postId: 1,
    User: {
      id: 'elonmusk',
      nickname: 'Elon Musk',
      image: '/yRsRRjGO.jpg',
    },
    content: '클론코딩 라이브로 하니 너무 힘들어요 ㅠㅠ',
    createdAt: new Date(),
    Images: [] as any[],
  }
  if (Math.random() > 0.5 && !noImage) {
    target.Images.push(
      {imageId: 1, link: faker.image.urlLoremFlickr()},
      {imageId: 2, link: faker.image.urlLoremFlickr()},
      {imageId: 3, link: faker.image.urlLoremFlickr()},
      {imageId: 4, link: faker.image.urlLoremFlickr()},
    )
  }

  return (
    <PostArticle post={target}>
      <div className={style.postWrapper}>
        <div className={style.postUserSection}>
          <Link href={`/${target.User.id}`} className={style.postUserImage}>
            <img src={target.User.image} alt={target.User.nickname}/>
            <div className={style.postShade}/>
          </Link>
        </div>
        <div className={style.postBody}>
          <div className={style.postMeta}>
            <Link href={`/${target.User.id}`}>
              <span className={style.postUserName}>{target.User.nickname}</span>
              &nbsp;
              <span className={style.postUserId}>@{target.User.id}</span>
              &nbsp;
              ·
              &nbsp;
            </Link>
            <span className={style.postDate}>{dayjs(target.createdAt).fromNow(true)}</span>
          </div>
          <div>{target.content}</div>
          <div>
            <PostImages post={target} />
          </div>
          <ActionButtons/>
        </div>
      </div>
    </PostArticle>
  )
}
```

### Suspense 컴포넌트
Suspense는 React Component 내부에서 비동기적으로 다른 요소를 불러올 때 해당 요소가 불러와질 때까지 Component의 렌더링을 잠시 멈추는 용도로 사용할 수 있는 컴포넌트이다.

<img width="650" alt="스크린샷 2024-01-05 오후 5 48 06" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/9d06f3f8-5452-4596-a4e9-7353db70d1b0">

Suspense를 사용해서 컴포넌트 내부에서 비동기적 데이터가 불러와지는 중에는 Suspense의 fallback Prop을 통해 loading.tsx 컴포넌트를 화면에 보여준다.

**suspense 컴포넌트 사용시 장점** </br>
서버에서 로딩이 필요한 부분과 로딩이 필요하지 않은 부분을 나누어 로딩이 필요하지 않은 부분만 클라이언트로 보내주고 로딩이 필요한 경우에는 로딩 후 클라이언트로 보내주는 스트리밍 방식으로 여러번 클라이언트로 보내줄 수 있다.
로딩이 필요한 부분만 서스펜스 컴포넌트로 감싸주는 식으로 분리 하여 사용 할 수 있다.

**세가지 로딩 시점 구분**

- page가 로딩 될때의 로딩 컴포넌트
- suspense가 로딩 될때의 로딩
- reace-query에서 api를 호출할때의 로딩

상위를 Suspense 컴포넌트로 감싸주고 자식 컴포넌트에서 useSuspenseQuery 또는 useSuspenseInfinityQuery를 사용하면 상위 Suspense 컴포넌트의 fallback의 loading ui를 사용 할 수 있다.


### errorBoundary 컴포넌트(서버컴포넌트의 에러에 대응)

errorBoundary 하위 컴포넌트 트리의 어디에서든 자바스크립트 에러를 기록하며 깨진 컴포넌트 트리 대신 폴백 UI를 보여주는 React 컴포넌트이다.
Error Boundary는 React Component 내부에서 에러가 발생한 경우 사용자에게 잘못된 UI나 빈 화면을 보여주는 대신 미리 정의해 둔 Fallback UI를 화면에 보여주기 위한 컴포넌트이다.

폴더 경로에 error.tsx를 만들어 주고 errorBoundary 컴포넌트의 fallback 컴포넌트를 연결해 준다.
<img width="653" alt="스크린샷 2024-01-05 오후 5 50 48" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/2273dde5-c195-4368-a375-87152dd51072">
<img width="652" alt="스크린샷 2024-01-05 오후 5 58 54" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/b54dd14a-cd8a-43c2-91d7-2d470ea79d02">

### suspense와 errorBoundary 컴포넌트를 함께 사용시

```shell
const User = () => (
  <ErrorBoundary FallbackComponent={UserProfileFallback}>
    <Suspense fallback={<UserProfileLoading />}>
      <UserProfile />
    </Suspense>
  </ErrorBoundary>
);

export default User;
```

### 구글 폰트 사용

📄 최상위 layout.tsx

```shell
import { Inter } from 'next/font/google'
 
const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  variable: '--font-roboto-mono',
  display: 'swap',
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={roboto_mono.variable}>
      <body>
        <h1>My App</h1>
        <div>{children}</div>
      </body>
    </html>
  )
}
```

📄 test.module.css
css 파일에서 `var(--font-roboto-mono)` 선언하여 사용

```shell
h1 {
  font-family: var(--font-roboto-mono);
}
```

# 사용된 라이브러리

### next-auth@5

```shell
npm install next-auth@5 @auth/core
```
- auth.ts, middleware.ts, app/api/auth/[...nextauth]/route.ts 생성
- 로그인을 위해 signIn("credentials") 호출(csrf 토큰 알아서 관리)
- 로그아웃을 위해 signOut 호출
- 클라이언트에서 내 정보 가져올 때는 useSession(), 서버에서는 await auth();
- session 안 내 정보는 email, name, image만 가능(헷갈리니 주의)

### 페이지 접근 권한
middleware.ts로 페이지 접근 제어

- (afterLogin) 내부의 [username]과 [username]/status/[id] 페이지는 모두 공개
- 그 외 (afterLogin) 페이지들은 로그인한 사람만 접근 가능

### Auth.js

로그인과 로그아웃시 CredentialsProvider 와 NextAuth.js 사용
- 로그인
- 로그아웃
- 현재 내 정보 불러오기

### 1. src 폴더내에 auth.js 생성

https://next-auth.js.org/providers/credentials

로그인시 CredentialsProvider의 함수가 호출된다.
pages에 로그인 페이지 라우터를 등록 해주도록 한다.
     
```shell
import NextAuth from "next-auth"
import CredentialsProvider from "next-auth/providers/credentials";
import {NextResponse} from "next/server";


export const {
  handlers: { GET, POST }, // auth에서 제공해주는 api 라우트  
  auth, //auth() 함수를 호출하면 내가 로그인을 했는지 안했는지 알아 낼수 있다.
  signIn, //로그인 하는용
} = NextAuth({
  pages: {
    signIn: '/i/flow/login', //로그인 라우터 
    newUser: '/i/flow/signup', //회원가입 페이지 라우터
  },
  providers: [
    CredentialsProvider({
      async authorize(credentials) {
        const authResponse = await fetch(`${process.env.AUTH_URL}/api/login`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            id: credentials.username,
            password: credentials.password,
          }),
        })

        if (!authResponse.ok) {
          return null
        }

        const user = await authResponse.json()
        console.log('user', user);
        return {
          email: user.id,
          name: user.nickname,
          image: user.image,
          ...user,
        }
      },
    }),
  ]
});
```
### 2. src 폴더내에 middleware.ts 생성

아래 등록되어진 라우터들의 페이지에 접속하기 전에 아래 middleware함수가 먼저 실행 된다. </br>
예) 로그인했으면 통과 로그인을 안했으면 redirect로 로그인 페이지로 연결

```shell
import { auth } from "./auth"
import {NextResponse} from "next/server";

export async function middleware() {

  로그아웃 후 로그인이 되어야 하는 페이지 주소를 입력하면 로그인 페이지로 이동하도록 하기
- session이 없을 경우 로그인 페이지로 이동
  const session = await auth();
  if (!session) {
    return NextResponse.redirect('http://localhost:3000/i/flow/login');
  }
}

// matcher은 미들웨어를 적용할 라우터들이며 공통적으로 로그인을 해야만 접근이 가능한 라우터들이다.
export const config = {
  matcher: ['/compose/tweet', '/home', '/explore', '/messages', '/search'],
}
```

### api 라우터 실제 경로 설정 
api 라우터는 브라우저 라우터처럼 실제 주소가 된다.

<img width="203" alt="스크린샷 2023-12-08 오후 5 25 30" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/25bbdaee-d378-4c84-8ec7-7f3390f5c806">

위 경로중 [...folderName]는 catch all route 이다. 
[...folderName] 경로에는 어떤 경로든 들어갈 수 있다. 

|Route|Example|URL|params|
|---|---|---|---|
|pages/shop/[...slug].js|	/shop/a|	{ slug: ['a'] }|
|pages/shop/[...slug].js|	/shop/a/b|	{ slug: ['a', 'b'] }|
|pages/shop/[...slug].js|	/shop/a/b/c|	{ slug: ['a', 'b', 'c'] }|

https://nextjs.org/docs/pages/building-your-application/routing/dynamic-routes#catch-all-segments

router.ts 

```shell
export { GET, POST } from '@/auth';
```
GET, POST의 함수를 구현하지 않고 auth에서 제공하는 GET, POST를 사용 한다. 


로그인 페이지에서 아이디와 페스워드 등을 입력호 onSubmit을 호출해줄때 signIn을 사용해 준다.

```shell

import {signIn} from "next-auth/react"; // 프론트엔드 서버 사용시
import {signIn} from "@/auth"; //서버 컴포넌트에서 사용

const onSubmit: FormEventHandler<HTMLFormElement> = async (e) => {
    e.preventDefault();
    setMessage('');
    try {
      await signIn("credentials", {
        username: id,
        password,
        redirect: false, //redirect를 true로 하면 서버에서 redirect를 실행 한다.
      })
      router.replace('/home');
    } catch (err) {
      console.error(err);
      setMessage('아이디와 비밀번호가 일치하지 않습니다.');
    }
  };
  const onClickClose = () => {
    router.back();
  };
```


### 이벤트 캡쳐링 onClickCapture
사용 컴포넌트 내에서 <Link/> 컴포넌트와 onClick이 동시에 사용되어 이벤트가 상위나 하위로 옮겨 가는 경우에 아래와 같이 사용할 수 있다.

- Capturing Phase (캡처링 단계): 이벤트가 가장 먼 조상 요소에서 시작하여 이벤트가 발생한 요소까지 이동
- Target Phase (타겟 단계): 이벤트가 실제로 발생한 대상 요소에서 이벤트 핸들러가 호출
- Bubbling Phase (버블링 단계): 이벤트가 다시 가장 먼 조상 요소로 거슬러 올라가면서 이벤트 핸들러가 호출


```shell
  <article onClickCapture={onClick}/>
```

### Dayjs 라이브러리

JavaScript에서 날짜 및 시간을 다루기 위한 경량 라이브러리입니다.

```shell
npm install dayjs
```

 

### 현재 날짜로부터 특정 날짜까지의 시간 간격

```shell

  const dayjs = require('dayjs');
  const relativeTime = require('dayjs/plugin/relativeTime');
  const 'dayjs/locale/ko'; 

  // 한글 플러그인 활성화
  dayjs.locale('ko');
  // 상대적인 시간 표시를 위한 플러그인 활성화
  dayjs.extend(relativeTime);
  

  // 특정 날짜
  const someDate = dayjs('2023-01-01');
  
  // 현재 날짜로부터 특정 날짜까지의 시간 간격을 사람이 읽기 쉬운 형식으로 얻기
  const timeAgo = someDate.fromNow();
  
  console.log(timeAgo); // 예: "2 months ago"
```



### 현재 날짜 및 시간 객체 생성 날짜 및 시간 지정 

- 포맷 지정

```shell
  date.format(); // 2022-05-24T10:30:25+09:00
  date.format("YY-MM-DD"); // 22-05-24
  date.format("DD/MM/YY"); // 05/24/22
  date.format("YYYY.MM.DD HH:mm:ss"); // 2022.05.24 10:30:25
  date.format("YYYY년MM월DD일 HH:mm:ss") // 2022년05월24일 10:30:25
```
</br>

- 요일
  
```shell
  dayjs.locale('ko');
  dayjs(2022-11-13T18:08:33).format('MM.DD(ddd)') // 11.13(월)
```
</br>

- 날짜 시간 단위 값 구하기

```shell

const now = dayjs();

  now.format(); // 2022-05-24T19:03:02+09:00
  now.get("year"); // 2022 (년)
  now.get("y"); // 2022 (년)

  now.get("month"); // 05 (월 - 0~11)
  now.get("M"); // 5 (월 - 0~11)

  now.get("date"); // 24 (일)
  now.get("D"); // 24 (일)

  now.get("day"); // 0 (요일 - 일요일 : 0, 토요일 : 6)
  now.get("d"); // 0 (요일 - 일요일 : 0, 토요일 : 6)

  now.get("hour"); // 19 (시)
  now.get("h"); // 19 (시)

  now.get("minute"); // 3 (분)
  now.get("m"); // 3 (분)

  now.get("second"); // 2 (초)
  now.get("s"); // 2 (초)

  now.get("millisecond"); // 179 (밀리초)
  now.get("ms"); // 179 (밀리초)
```
</br>

- 날짜 및 시간 더하기 빼기 - add(), subtract()
  
```shell
  const date = dayjs("2022-05-24 10:30:21");
  date.add(1, "year").format(); // 2023
  date.add(1, "month").format(); // 2022-06
  date.add(1, "week").format(); // 2022-05-31

  date.add(1, "year").format(); // 2021
  date.add(1, "month").format(); // 2022-04
  date.add(1, "week").format(); // 2022-05-17
```
</br>



### 조건부에 따른 클래스 합성 라이브러리 cx

변수를 만들고 변수의 boolean 상태 값이 true일때 해당 스타일이 적용 되도록 한다.

```shell

"use client"
import style from './post.module.css';
import cx from 'classnames';

export default function ActionButtons() {
  //아래 변수의 상태 값이 true일때 해당 스타일이 적용 되도록 한다.
  const commented = true;
  const reposted = true;
  const liked = false;

  return (
    <div className={style.actionButtons}>
      <div className={cx(style.commentButton, { [style.commented]: commented })}></div>
      <div className={cx(style.repostButton, reposted && style.reposted)}></div>
      <div className={cx([style.heartButton, liked && style.liked])}></div>
    </div>
  )
}
```

css 사용 예)

```shell
  .heartButton.liked svg, .heartButton:hover svg {
      fill: rgb(228, 34, 126);
  }
  
  .heartButton.liked .count, .heartButton:hover .count {
      color: rgb(228, 34, 126);
  }
```





### .env
- 브라우저에서 접근 가능한 환경 변수는 NEXT_PUBLIC 으로 시작하면 된다.

```shell
NEXT_PUBLIC_API_MOCING = enabled;
```
- NEXT_PUBLIC으로 시작하지 않을 경우에는 서버에서만 접근 가능하다.

```shell
NEXT_PUBLIC_API_MOCING = enabled;
```



# [참고] css 가운데 정렬 

- 하나의 컨테이너 안에 좌우 엘레멘트 두개를 두고 가운데 정렬 하는 경우 여백이 좌우 넓이가 알맞게 브라우저에 반응하는 CSS 

<img src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/1feb979a-326a-4781-8040-2230b047b9e0" width="720px" border="1px" border="1px solid #000"/>

<img src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/1892a86a-f051-4116-ae95-90141577d9d7" width="720px" border="1px solid #000"/>
</br>
</br>

```shell

.container{
  display:flex;
  align-items:strech;
}

.left-section-wrapper{
  display:flex;
  align-items:flex-end;
  flex-direction:column;
  flex-grow:1
}

.right-section-wrapper{
  display:flex;
  align-items:flex-start;
  flex-direction:column;
  flex-grow:1
}

```
</br>
</br>

# [참고] css trasition 사용

```shell
  .uploadButton {
      width: 34px;
      height: 34px;
      display: flex;
      align-items: center;
      justify-content: center;
      border: none;
      cursor: pointer;
      border-radius: 17px;
      transition-duration: 0.2s;
      transition-property: background-color;
      background-color: rgb(29, 155, 240, 0.01);
     
  }
  .uploadButton:hover {
      background-color: rgb(29, 155, 240, 0.1);
  }
```
마우스 hover, out시 transition-property의 background-color값이 변경될때 마다 0.2s의 시간을 두고 자연스럽게 변경 되도록 한다.
</br>
</br>
# align-items : strech; 
stretch는 디폴트 값이며, 모든 아이템이 컨테이너의 크기에 맞춰서 늘어난다.
<img width="813" alt="스크린샷 2023-12-13 오후 3 33 18" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/cb40b9f7-a108-4ae5-8cb5-ccd1230b4a30">
</br>
</br>
# align-content
align-content는 flex-wrap과 관련된 속성으로 아이템이 flex-wrap에 의해 두 줄 이상으로 배열되었을 경우의 배치를 컨트롤하는 속성이다.
</br>
</br>
<img width="804" alt="스크린샷 2023-12-13 오후 3 37 43" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/77e17b1e-b609-46d5-90e4-f8de5cb593b9">

- align-content의 속성

  - stretch: 디폴트 값으로, 부모 요소의 보조 축의 전체 영역을 다 차지한다.
  - center: 부모 요소의 보조 축의 중앙에 정렬된다.
  - flex-start: 부모 요소의 보조 축의 시작 부분에 정렬된다.
  - flex-end: 부모 요소의 보조 축의 끝 부분에 정렬된다.
  - space-between: 부모 요소의 보조 축의 양 끝에 정렬되며, 동일한 거리 간격을 가진다.
  - space-around: 부모 요소의 보조 축의 양 끝에 정렬되지만, space-between과 다르게 양 끝이 약간 떨어져 있다.
  - space-evenly: 부모 요소의 보조 축에 정렬될 때 아이템들이 동일한 양쪽 마진 값을 가진다.

</br>
</br>

# display: grid

```shell
.fourImage {
    height: 272px;
    display: grid;
    grid-template-rows: 1fr 1fr;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
}
```

# 그외에 사항들 체크 사항

- dvw와 dvh의 단위를 사용하여 css로 페이지의 반응형 넓이 높이를 넣어준다. 이점은 페이지에서 주소창이 사라지거나 생기는 경우에 화면에서 깨지지 않고 모두 대응이 가능하다.
- next.js에서는 a태그 대신 Link 태그를 이용한다. (a태그는 클릭시 페이지가 새로 고침 되는 코딩을 해서는 안된다.)
- 서버컴포넌트 리다이렉트 > redirect(라우트경로) 기능이 next13버전에서 추가되었다. 서버 컴포넌트에서 redirect 시켜줄 수 있다. import {redirect} from "next/navigation";
- 클라이언트에서 리다이렉트 > const router = useRouter(); router.replace('라우터 경로'); 해주면 클라이언트 컴포넌트에서 리다이렉트 시켜줄수 있다.
- <Image/>를 next에서 지원해 준다. <Image/>는 next에서 알아서 이미지최적화를 해준다. 이미지 경로는 import Logo from "public 폴더 안에 이미지 경로"로 사용한다.
- \<Link/> 태그는 a태그 이므로 a태그 일때는 display: inline-block; 로 해주는 것이 좋다.
- 엘레멘트에 blur 필터주기 : backdrop-filter : blur(12px);
  




  

[참고]
- https://www.inflearn.com/course/lecture?courseSlug=next-react-query-sns%EC%84%9C%EB%B9%84%EC%8A%A4&unitId=194471&tab=curriculum
[참고]
- https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming
- https://nextjs.org/docs/app/building-your-application/routing/error-handling
