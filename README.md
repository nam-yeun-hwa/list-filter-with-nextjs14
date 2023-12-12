# 기술 스택
- next.js 14.0.4
- typescript

# Next.js 13 버전에서 달라진 점

Next.js 앱의 컴포넌트와 페이지에 대해 생각하는 방식에 큰 패러다임 변화를 가져왔습니다. 
Next.js 13 버전 이후부터는 Using App Router와 Using Pages Router을 제공 합니다.
기존버전의 경우는 Pages Router를 사용했다면 13버전 이후로 새롭게 App Router가 추가 되었습니다.

## App Router 뭔가요? </br>
App 디렉터리는 Next.js에서 라우트를 처리하고 뷰를 렌더링하기 위한 새로운 전략입니다. 

기존의 Next.js에서는 여러 파일에 나누어진 페이지를 생성하고 매핑하는 방식을 사용했습니다. 이로 인해 복잡성이 증가할 수 있었습니다. App Router는 페이지 구조를 더 단순화하고 일관성을 증가시킴으로써 개발자 경험을 향상시킬 수 있습니다.
App Router는 페이지 및 라우팅 로직을 통합하여 페이지 간 이동을 보다 효율적으로 관리할 수 있게 합니다. 이로써 라우팅 구성이 더욱 직관적이고 관리하기 쉬워집니다.</br></br>

## Next App Router
가장 크게 다른 점

- 각종 폴더 유형 추가로 디렉토리 라우팅이 편해짐
- 레이아웃 기능
- 페이지별 권한 체크
- 서버 컴포넌트 분리로 인한 최적화
- 데이터 캐시
- 서버 액션

## 서버 컴포넌트를 적극적으로 사용
react18 버전을 사용하는 nextjs13은 서버 컴포넌트를 적극적으로 사용합니다. </br>
서버 컴포넌트를 설명 하자면 넥스트 서버에서 리액트를 미리 렌더링해서 프론트 또는 브라우저로 완성된 HTML를 미리 보내줍니다. 사용자의 브라우저에서 했던 일들을 넥스트 서버에서 처리하기 떄문에 넥스트 서버의 부담이 늘어 났고 그 부담을 줄이기 위해 넥스트 서버에서는 적극적으로 캐싱을 활용하여 부담을 줄이도록 하였습니다. 예전에 Pages Router를 사용했을때 각각의 페이지에서 캐싱 기능을 구현하였다면 App Router에서는 캐싱등의 기능을 넥스트 서버에서 대신하여 처리해 줍니다.

예) page.tsx, layout.tsx


# src 폴더 설명
- layout.tsx : 상위 header, bottom등 공통으로 위치하는 컴포넌트들을 배치해 준다.
  - 페이지 내부 app에 들어있는 페이지 안에서는 따로 layout.tsx를 생성하여 사용하면 페이지당 공통 레이아웃이 가능하다.
  - globals.css 공통적으로 사용하는 css를 app가 사용하는 layout.tsx에 넣어준다.
    
- app : Next App Router사용. 주소와 관련 있는 파일들이 위치한다.
  - app 폴더 안에서 다이나믹 라우팅(slug)를 지원해 준다. </br>
  - 사용법은 [다이나믹라우팅 이름] 으로 폴더명으로 넣어준다.
  - 예) [username]/status/[id] 라는 주소는 아래와 같이 경로로 만들어 준다.
 
<img width="203" alt="스크린샷 2023-12-08 오후 5 25 30" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/b414bbc8-b79e-4af5-8e3a-79034ff0857d">


- 각각 username,id값으로 들어온 주소에 대해 page.tsx로 렌더링 된다.
- app의 경로에 해당하지 않는 페이지는 not-found.tsx로 대응하여 페이지를 적용해 줄수 있다. 폴더 경로는 src에 위치


<img width="268" alt="스크린샷 2023-12-08 오후 8 57 13" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/b0147a5f-eb00-4821-90ed-cc692a397a98">

## Routing Group
- 상태에 따른 폴더 이름으로 카테고리(레이아웃) 나누기용 
- 실제 주소에는 관여하지 않는다.
- 소괄호를 사용하여 폴더이름을 지정한다.

예) (beforelogin)
    (afterlogin)

공통 레이아웃이 다를경우 페이지를 나눠줄 수 있고 
beforelogin의 경우에 layout.tsx를 생성하여 레이아웃을 지정해 줄수도 있다.


# 그외에 사항들 체크 사항

- dvw와 dvh의 단위를 사용하여 css로 페이지의 반응형 넓이 높이를 넣어준다. 이점은 페이지에서 주소창이 사라지거나 생기는 경우에 화면에서 깨지지 않고 모두 대응이 가능하다.
- next.js에서는 a태그 대신 Link 태그를 이용한다. (a태그는 클릭시 페이지가 새로 고침 되는 코딩을 해서는 안된다.)
- 서버컴포넌트 리다이렉트 > redirect(라우트경로) 기능이 next13버전에서 추가되었다. 서버 컴포넌트에서 redirect 시켜줄 수 있다. import {redirect} from "next/navigation";
- 클라이언트에서 리다이렉트 > const router = useRouter(); router.replace('라우터 경로'); 해주면 클라이언트 컴포넌트에서 리다이렉트 시켜줄수 있다.
- <Image/>를 next에서 지원해 준다. <Image/>는 next에서 알아서 이미지최적화를 해준다. 이미지 경로는 import Logo from "public 폴더 안에 이미지 경로"로 사용한다.

# next13에서 추가된 라우트 기능
### Parallel Route <br/>
  page.tsx 두개를 동시에 보여주고 싶을때 사용한다. (기존 페이지를 배경으로 두고 모달을 새루운 경로로 띄울때 사용했다.) <br/>
  아래처럼 page.tsx와 @modal 루트경로가 같아야 한다. <br/>
  
  <img width="239" alt="스크린샷 2023-12-08 오후 10 10 43" src="https://github.com/nam-yeun-hwa/list-filter-with-nextjs14/assets/138950568/2e3dc802-f9e8-4092-aaac-b3b7a1ed6498">
  
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

### 서버 컴포넌트에서는 useState, useEffect등을 사용할 수 없다. 

page.tsx, layout.tsx 등은 모두 서버 컴포넌트이기 때문에 화면에 렌더링 된후에 사용하는 useState나 useEffect를 사용할 수 없다. <br/>
이런 경우에는 서버 컴포넌트를 클라이언트 컴포넌트도 변경 해주어야 한다. <br/>
클라이언트 컴포넌트로 변경하는 방법은 page.tsx나 layout.tsx 상단에 "use client"; 를 추가해 주면 된다. <br/>

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
app폴더 하위 (beforeLogin)폴더 내부에 _component폴더를 사용하여 내부에 컴포넌트를 만들면 실제 주소에는 관여하지 않고 공통되는 페이지를 화면에 보여 줄수 있다. 
@modal/(..)i/flow/login/page.tsx 와 i/flow/login/page.tsx는 결국 같은 내용을 화면에 렌더링 하는데 이럴때는 같은 (beforeLogin)을 두고 _component를 두어 각 페이지에서 하나의 컴포넌트를 import하여 사용하면 된다.
결국 실제 라우터에 관여 하지 않는 폴더는 (폴더이름), _component 의 두가지 경우가 있다.


### useSelectedLayoutSegment
현재 나의 라우터 위치를 알아낼 수 있는 훅을 next에서 지원해 준다.
useSelectedLayoutSegment는 서브 컴포넌트가 아닌 클라이언트 컴포넌트에서 가능 하다.

```shell
const segment = useSelectedLayoutSegment();
console.log(segment) // 현재 활성화된 상위 라우터 주소

const segment = useSelectedLayoutSegments();
console.log(segment) // 현재 활성화된 상위, 하위 라우터 주소 ['compose', 'tweet'];
```


# 참고 CSS 가운데 정렬 

- 하나의 컨테이너 안에 좌우 엘레멘트 두개를 두고 가운데 정렬 하는 경우 여백이 좌우 넓이가 알맞게 브라우저에 반응하는 CSS 

<img src="https://private-user-images.githubusercontent.com/138950568/289355427-1892a86a-f051-4116-ae95-90141577d9d7.jpg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDIyMDg4MzAsIm5iZiI6MTcwMjIwODUzMCwicGF0aCI6Ii8xMzg5NTA1NjgvMjg5MzU1NDI3LTE4OTJhODZhLWYwNTEtNDExNi1hZTk1LTkwMTQxNTc3ZDlkNy5qcGc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBSVdOSllBWDRDU1ZFSDUzQSUyRjIwMjMxMjEwJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDIzMTIxMFQxMTQyMTBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1jMmQ0ZTU4N2I5MmE1MDI5Y2UwNmU3Yjg0NGNhM2FlNGYwYmQ5MzYwZmQ2NzFlNGIxY2VkNjBlNDZlZWQ5NDY4JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.7txnnZUq09__sffqLpDejFVRah6dsPUVMTWoJNr5AB8" width="720px" border="1px" border="1px solid #000"/>

<img src="https://private-user-images.githubusercontent.com/138950568/289354666-1feb979a-326a-4781-8040-2230b047b9e0.jpg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDIyMDgzMTksIm5iZiI6MTcwMjIwODAxOSwicGF0aCI6Ii8xMzg5NTA1NjgvMjg5MzU0NjY2LTFmZWI5NzlhLTMyNmEtNDc4MS04MDQwLTIyMzBiMDQ3YjllMC5qcGc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBSVdOSllBWDRDU1ZFSDUzQSUyRjIwMjMxMjEwJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDIzMTIxMFQxMTMzMzlaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yNTkyYjZjNzFjNWUwNWYwM2Y0MDQ2OTQ3NTBmNjY3YzhkODhkMDNlNDg3YzhhYmQ0YWNjYTJkNjkxYTVjMDZjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.FZw47_sElKVdwx2AcwKZ3ikKgIyAvuMvGZITOj94JWk" width="720px" border="1px solid #000"/>



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

# globals.css 사용예시


- \<Link/> 태그는 a태그 이므로 a태그 일때는 display: inline-block; 로 해주는 것이 좋다.




  


