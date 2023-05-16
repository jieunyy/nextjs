# Dynamic Routes
https://nextjs.org/learn/basics/dynamic-routes
<br/><br/>
## Page Path Depends on External Data
각 페이지 경로가 외부 데이터에 의존하는 경우, Next.js를 사용하면 외부 데이터에 의존하는 경로가 있는 페이지를 정적으로 생성할 수 있다. 이는 동적 URL을 활성화한다.
<br/>
## Overview of the Steps
/posts/<id>에 해당하는 동적 URL을 활성화하기 위해서는,
1. **[id].js** 페이지를 만든다. [로 시작하고 ]로 끝나는 페이지는 Next.js의 동적 경로이다.
2. 이 페이지에서 **getStaticPaths**라는 비동기 함수를 내보낸다. 함수에서는 id에 대한 가능한 값 목록을 반환해야 한다. 
* *이때, 반환된 목록은 단순히 문자열 배열이 아니라 개체 배열이어야 한다. 파일 이름에 [id]를 사용하고 있기 때문에 각 개체에는 매개 변수 키가 있고 ID 키가 있는 개체가 포함되어 있어야 한다. 그렇지 않으면 getStaticPaths가 실패하게 되기 때문이다.*
3. 마지막으로, **getStaticProps를 다시 구현**한다.
<br/>
## Quick Review: 
  How does params.id from getStaticProps({ params }) know the key is named id?
  > The value from the file name

<br/>
 <hr/>
<br/>
 
## Render Markdown
마크다운 콘텐츠를 렌더링할 때는 **remark**라이브러리를 사용한다.
라이브러리 설치 후, lib/posts.js에 import문을 추가한다.
그 다음 getPostData() 함수를 업데이트한다.
* **중요**: wait for remark를 사용해야 하기 때문에 async 키워드를 getPostData에 추가한다. 
* ***async/wait를 사용하면 비동기적으로 데이터를 가져올 수 있다.***
* getPostData를 호출하기 위해 await을 이용하기 위해서는 pages/posts/[id].js의 getStaticProps에도 await 키워드를 추가해주어야 한다.
  
마지막으로, **dangerouslySetInnerHTML**을 사용하여 contentHtml을 렌더링하기 위해 pages/posts/[id].js의 Post 컴포넌트를 업데이트한다.
  
<br/>
 <hr/>
<br/>
  
## Polishing the Post Page
1. **Adding title to the Post Page**: next/head을 import하여 <Head> 태그를 사용한다.
2. **Formatting the Date**: date-fns 라이브러리를 사용하여 Date 컴포넌트를 만든다.
    * Note: 자세한 사항은 date-fns 웹사이트에서 참고
3. **Adding CSS**: 기존에 구성해둔 styles/utils.module.css의 css를 사용한다.
4. **Polishing the Index Page**: Link와 date컴포넌트를 import해 사용한다.
  
<br/>
 <hr/>
<br/>
  
## Dynamic Routes Details
1. **Fetch External API or Query Database**: getStaticProps와 마찬가지로 getStaticPaths는 모든 데이터 소스에서 데이터를 가져올 수 있다. 일례로, getAllPostIds(getStaticPaths에서 사용됨)는 외부 API에서 소스를 가져올 수 있다
2. **Development vs. Production**: 
  * 개발(npm run devor yarn dev)에서는 모든 요청에 대해 getStaticPaths가 실행된다.
  * 프로덕션에서는 빌드 시 getStaticPaths를 실행한다.
3. **Fallback**
getStaticPaths에서 false라는 폴백을 반환한 경우, 즉 폴백이 거짓인 경우 **getStaticPaths에서 반환되지 않는 경로는 404페이지**가 된다.
한편, **폴백이 참이면 getStaticProps의 동작이 변경**된다.
  * getStaticPaths에서 반환된 경로는 빌드 시에 HTML로 렌더링된다.
  * 빌드 시 생성되지 않은 경로는 404페이지가 되지 않는다. 대신 Next.js는 이러한 경로에 대한 첫 번째 요청 시 페이지의 "fallback" 버전을 제공한다.
  * 백그라운드에서 Next.js는 요청된 경로를 정적으로 생성한다. 빌드 시 미리 렌더링된 다른 페이지와 마찬가지로 동일한 경로에 대한 후속 요청은 생성된 페이지에 서비스로 작용한다.
폴백이 차단되었을 경우 새 경로는 getStaticProps로 서버 측에서 렌더링되고 이후 요청을 위해 캐시되므로 경로당 한 번만 발생한다.
4. **Catch-all Routes**: 동적 경로는 **브래킷 내부에 점 3개(...)를 추가**하여 모든 경로를 포착하도록 확장할 수 있다. pages/pages/[...id].js는 /pages/a와 일치하지만 /pages/a/b, /pages/a/b/c 등도 마찬가지이다.
5. **Router**: Next.js router에 접근하려면 next/router에서 useRouter 훅을 임포트해 사용한다.
6. **404 Pages**: pages/404.js을 만들어 404 페이지를 커스텀할 수 있다. 이 페이지는 빌드 타임에 정적으로 실행된다.
