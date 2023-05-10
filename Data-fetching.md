(about)Next.js 공식문서를 번역하며 공부합니다.

# Data-fetching
https://nextjs.org/learn/basics/data-fetching

## Pre-rendering
Next.js는 기본적으로 모든 페이지를 렌더링한다. Next.js는 클라이언트측의 자바스크립트에만 의존하지 않고, 각 페이지에서 미리 HTML를 생성할 수 있다. Pre-rendering은 성능과 SEO를 최적화한다.

형성된 각 HTML은 페이지에 필요한 JS 코드와 연결된다. 브라우저에서 페이지를 로드하면 JS 코드가 실행되어 페이지가 완전히 활성화된다.(이것이 hydration)

*chrome에서 js를 disable하므로써 Pre-rendering 동작을 확인할 수 있다.*

<br/>

## Pre-rendering vs No Pre-rendering
* **Pre-rendering**: (nitial load(js가 로드되기 전 HTML이 Pre-rendered) -> js loads -> Hydration(<Link/>와 같은 interactive components)

* **No Pre-rendering(Plain React.js)**: no initial load -> Hydration

<br/>

## Two Forms of Pre-rendering
Next.js에는 두 종류의 pre-rendering 방식이 있다. 차이는 HTML 생성 시기에 있다.
* **Static Generation**는 **build time**에 HTML을 생성(once)하고 각 요청마다 재사용한다.
* **Server-side Rendering(ssr)**는 **(every)요청 발생 후** HTML을 생성한다.

<br/>

## Per-page Basis
Next.js는 페이지별로 다른 pre-rendering 방식을 적용할 수 있다.

<br/>

## When to Use Static Generation v.s. Server-side Rendering
데이터 유무에 관계없이, **Static Generation**을 권장한다. 정적으로 빌드한 페이지를 CDN으로 캐시해두면 성능에 도움이 되기 때문이다. 
일례로 홍보목적의 페이지, 블로그 게시글, 이커머스 상품 리스트, 공식 문서 및 서비스 페이지에 적합하다.

그러나 최신 정보가 유의미하게 작용하는 페이지에서는 **Server-side Rendering**을 사용할 것을 권장한다. pre-rendering 작업을 제하고 클라이언트측 js만을 통해 정보를 가져올 수도 있기 때문이다.
일례로 사용자에 따라 데이터가 달리진다면 해당 데이터는 SEO에 필요하지 않다. 따라서 미리 렌더링할 필요도 없기 때문에 Server-side Rendering을 이용한다.

<br/>
