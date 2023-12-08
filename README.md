# 기술 스택
- next.js 14.0.4
- typescript

# Next.js 13 버전에서 달라진 점

Next.js 앱의 컴포넌트와 페이지에 대해 생각하는 방식에 큰 패러다임 변화를 가져왔습니다. 
Next.js 13 버전 이후부터는 Using App Router와 Using Pages Router을 제공 합니다.
기존버전의 경우는 Pages Router를 사용했다면 13버전 이후로 새롭게 App Router가 추가 되었습니다.

## App 디렉터리가 뭔가요? </br>
App 디렉터리는 Next.js에서 라우트를 처리하고 뷰를 렌더링하기 위한 새로운 전략입니다. 

기존의 Next.js에서는 여러 파일에 나누어진 페이지를 생성하고 매핑하는 방식을 사용했습니다. 이로 인해 복잡성이 증가할 수 있었습니다. App Router는 페이지 구조를 더 단순화하고 일관성을 증가시킴으로써 개발자 경험을 향상시킬 수 있습니다.
App Router는 페이지 및 라우팅 로직을 통합하여 페이지 간 이동을 보다 효율적으로 관리할 수 있게 합니다. 이로써 라우팅 구성이 더욱 직관적이고 관리하기 쉬워집니다.</br></br>

## 서버 컴포넌트를 적극적으로 사용
react18 버전을 사용하는 nextjs13은 서버 컴포넌트를 적극적으로 사용합니다. </br>
서버 컴포넌트를 설명 하자면 넥스트 서버에서 리액트를 미리 렌더링해서 프론트 또는 브라우저로 완성된 HTML를 미리 보내줍니다. 사용자의 브라우저에서 했던 일들을 넥스트 서버에서 처리하기 떄문에 넥스트 서버의 부담이 늘어 났고 그 부담을 줄이기 위해 넥스트 서버에서는 적극적으로 캐싱을 활용하여 부담을 줄이도록 하였습니다. 예전에 Pages Router를 사용했을때 각각의 페이지에서 캐싱 기능을 구현하였다면 App Router에서는 캐싱등의 기능을 넥스트 서버에서 대신하여 처리해 줍니다.

예) page.tsx, layout.tsx


# src 폴더 설명
- layout.tsx : 상위 header, bottom등 공통으로 위치하는 컴포넌트들을 배치해 준다.
  - 페이지 내부 app에 들어있는 페이지 안에서는 따로 layout.tsx를 생성하여 사용하면 페이지당 공통 레이아웃이 가능하다.
- app : 주소와 관련 있는 파일들이 위치한다.
  - app 폴더 안에서 다이나믹 라우팅(slug)를 지원해 준다. </br>
  - 사용법은 [다이나믹라우팅 이름] 으로 폴더명으로 넣어준다.
  - 예) [username]/status/[id] 라는 주소는 아래와 같이 경로로 만들어 준다.
 
  <img src="https://private-user-images.githubusercontent.com/138950568/289009775-b414bbc8-b79e-4af5-8e3a-79034ff0857d.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDIwMjQzMjMsIm5iZiI6MTcwMjAyNDAyMywicGF0aCI6Ii8xMzg5NTA1NjgvMjg5MDA5Nzc1LWI0MTRiYmM4LWI3OWUtNGFmNS04ZTNhLTc5MDM0ZmYwODU3ZC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBSVdOSllBWDRDU1ZFSDUzQSUyRjIwMjMxMjA4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDIzMTIwOFQwODI3MDNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xZTA3ZWU4YWE4ZDU1ZDM5Mzc4NjYxNzE2Mjc2ZmJmZGUyZDQxZjVjYmM4NjNlOWJjM2ZjNmIwYWJjYjFkNTQ4JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.v97zMcv6pKhDD7XlhSNP3jHYdFYDm4dnYJucR0mdPuw" width="250px" height="auto"/>

  각각 username,id값으로 들어온 주소에 대해 page.tsx로 렌더링 된다.

- app의 경로에 해당하지 않는 페이지는 not-found.tsx로 대응하여 페이지를 적용해 줄수 있다. 폴더 경로는 src에 위치하면 된다. 

<img src="https://private-user-images.githubusercontent.com/138950568/289059983-b0147a5f-eb00-4821-90ed-cc692a397a98.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDIwMzcyMjcsIm5iZiI6MTcwMjAzNjkyNywicGF0aCI6Ii8xMzg5NTA1NjgvMjg5MDU5OTgzLWIwMTQ3YTVmLWViMDAtNDgyMS05MGVkLWNjNjkyYTM5N2E5OC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBSVdOSllBWDRDU1ZFSDUzQSUyRjIwMjMxMjA4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDIzMTIwOFQxMjAyMDdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02MmY4NmI5YzhmMDRlZjAxMjNkMzNiNTAwYzZiYmZmNGQwNjMwODdlNjJiM2VlNWY2ZmQxMGFiZTM1M2U4ZjBlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.JBbmizzo9jKisRvmjbEIM8Gna740yf8thYRmmODuDLo" width="250px" height="auto"/>

- 상태에 따른 폴더 이름으로 카테고리 나누기
(실제 주소에는 관여하지 않는다.)
(beforelogin)
(afterlogin)

레이아웃이 적용되고 안되고 등으로 폴더 경로 페이지를 나눠줄 수 있고 
beforelogin의 경우에 layout.tsx를 생성하여 레이아웃을 지정해 줄수도 있다.


# 그외에 사항들 체크 사항

- dvw와 dvh의 단위를 사용하여 css로 페이지의 반응형 넓이 높이를 넣어준다. 이점은 페이지에서 주소창이 사라지거나 생기는 경우에 화면에서 깨지지 않고 모두 대응이 가능하다.
- next.js에서는 a태그 대신 Link 태그를 이용한다. (a태그는 클릭시 페이지가 새로 고침 되는 코딩을 해서는 안된다.)
- redirect(라우트경로) 기능이 next13버전에서 추가되었다. import {redirect} from "next/navigation";
- <Image/>를 next에서 지원해 준다. <Image/>는 next에서 알아서 이미지최적화를 해준다. 이미지 경로는 import Logo from "public 폴더 안에 이미지 경로"로 사용한다.

## next13에서 추가된 라우트 기능
### Parallel Route <br/>
  page.tsx 두개를 동시에 보여주고 싶을때 사용한다. (기존 페이지를 배경으로 두고 모달을 새루운 경로로 띄울때 사용했다.) <br/>
  아래처럼 page.tsx와 @modal 루트경로가 같아야 한다. <br/>
  
  <img src="https://private-user-images.githubusercontent.com/138950568/289075554-2e3dc802-f9e8-4092-aaac-b3b7a1ed6498.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDIwNDE0MTIsIm5iZiI6MTcwMjA0MTExMiwicGF0aCI6Ii8xMzg5NTA1NjgvMjg5MDc1NTU0LTJlM2RjODAyLWY5ZTgtNDA5Mi1hYWFjLWIzYjdhMWVkNjQ5OC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBSVdOSllBWDRDU1ZFSDUzQSUyRjIwMjMxMjA4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDIzMTIwOFQxMzExNTJaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0wMjBjNDcwZDlkOTRlOTViYzk3OTE5MWE5ZDNmNTdlODcyYzYzZjVlN2VhNTQ0N2YxZWY3ZTEwMjIwYWQzNjBlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.nIcKuYk2VoEDbPv5Ok0HDohEnqHU9DOId3fR4f08aaU" width="250px" height="auto"/>
  
  그리고 layout.tsx에 modal을 props로 받아주어야 한다.
  
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
layout.tsx에서 page.tsx는 children으로 modal은 modal로 렌더링 된다.
@madal폴더에도 page.tsx가 필요한데 넣을 내용이 없을 경우에는 default.tsx를 추가 해주어야 오류가 뜨지 않는다. 

## 서버 컴포넌트에서는 useState, useEffect등을 사용할 수 없다. 

page.tsx, layout.tsx 등은 모두 서버 컴포넌트이기 때문에 화면에 렌더링 된후에 사용하는 useState나 useEffect를 사용할 수 없다. <br/>
이런 경우에는 서버 컴포넌트를 클라이언트 컴포넌트도 변경 해주어야 한다. <br/>
클라이언트 컴포넌트로 변경하는 방법은 page.tsx나 layout.tsx 상단에 "use client"; 를 추가해 주면 된다. <br/>

### Interception Routes <br/>


