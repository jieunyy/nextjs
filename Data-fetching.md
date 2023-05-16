(about)Next.js 공식문서를 번역하며 공부합니다.

# Data-fetching(데이터 가져오기)
https://nextjs.org/learn/basics/data-fetching

## Pre-rendering
Next.js는 기본적으로 모든 페이지를 렌더링한다. Next.js는 클라이언트측의 자바스크립트에만 의존하지 않고, 각 페이지에서 미리 HTML를 생성할 수 있다. Pre-rendering은 성능과 SEO를 최적화한다.

형성된 각 HTML은 페이지에 필요한 JavaScript 코드와 연결된다. 브라우저에서 페이지를 로드하면 JavaScript 코드가 실행되어 페이지가 완전히 활성화된다.(이것이 hydration)

*chrome에서 js를 disable하므로써 Pre-rendering 동작을 확인할 수 있다.*

<br/>

## Pre-rendering vs No Pre-rendering
* **Pre-rendering**: (nitial load(js가 로드되기 전 HTML이 Pre-rendered) -> js loads -> Hydration(<Link/>와 같은 interactive components)

* **No Pre-rendering(Plain React.js)**: no initial load -> Hydration

<br/>
<hr/>
<br/>

## Two Forms of Pre-rendering
Next.js에는 두 종류의 pre-rendering 방식이 있다. 차이는 HTML 생성 시기에 있다.
* Static Generation(정적 데이터 생성)는 **build time**에 HTML을 생성(once)하고 각 요청마다 재사용한다.
* Server-side Rendering(ssr)는 **(every)요청 발생 후** HTML을 생성한다.

<br/>

## Per-page Basis
Next.js는 페이지별로 다른 pre-rendering 방식을 적용할 수 있다.(hybrid)

<br/>

## When to Use Static Generation v.s. Server-side Rendering
데이터 유무에 관계없이, **Static Generation**을 권장한다. 정적으로 빌드한 페이지를 CDN으로 캐시해두면 성능에 도움이 되기 때문이다. 
일례로 홍보 목적의 페이지, 블로그 게시글, 이커머스 상품 리스트, 공식 문서 및 서비스 페이지에 적합하다.

그러나 최신 정보가 유의미하게 작용하는 페이지에서는 **Server-side Rendering**을 사용할 것을 권장한다. pre-rendering 작업을 제하고 클라이언트측 JavaScript만을 통해 정보를 가져올 수도 있기 때문이다.
일례로 사용자에 따라 데이터가 달리진다면 해당 데이터는 SEO에 필요하지 않다. 따라서 미리 렌더링할 필요도 없기 때문에 Server-side Rendering을 이용한다.

<br/>
<hr/>
<br/>

## Static Generation with and without Data
**Static Generation**는 **build time**에 HTML을 생성했었다. 이는 기본적으로 외부 데이터를 패칭하지 않고 작동한다.

그러나, 외부 데이터 패칭 없이 HTML을 렌더링할 수 없는 케이스도 존재한다. 
일례로, 
* 파일 시스템에 접근하거나
* 외부 API를 패칭하거나
* 데이터베이스를 쿼리하는 경우
이를 위해 Next.js는 데이터가 포함된 **Static Generation**을 즉시 지원한다.

<br/>

## Static Generation with Data using getStaticProps
**getStaticProps**은 빌드타임에 작동하는 비동기 함수이다.
해당 함수 내부에서, 외부 데이터를 패치해오거나 패칭한 데이터를 페이지에 prop으로 넘겨줄 수 있다.

<br/>
<hr/>
<br/>

## Implement getStaticProps(getStaticProps 구현)
* 파일 시스템에서 데이터를 패칭하는 경우, **getSortedPostsData**메소드를 사용한다.

<br/>

## Fetch External API or Query Database
* 외부 API를 사용하고자 하면 fetch 메소드를 사용할 수 있다.(import 필요 없음)
* 데이터베이스에 직접적으로 쿼리할 수 있다.
이는 **getStaticProps**가 서버측에서만 작동하기 때문이다. 클라리언트측에서 작동하거나, 브라우저 js 번들 내에 위치하지 않는다. 따라서, 브라우저로 정보를 가져오지 않고도 데이터베이스를 쿼리해 바로 보여줄 수 있다.

<br/>

## Development vs. Production
* **Development**단계에서, **getStaticProps**은 매 요청마다 실행된다.
* **Production**단계에서, **getStaticProps**는 빌드할 때 실행된다. 이때, getStaticPaths에서 반환한 폴백 키를 사용하여 이 동작을 향상시킬 수 있다.

<br/>

## Only Allowed in a Page
**getStaticProps**은 **page**에서만 export 가능하다.
이는 모든 페이지가 렌더링되기 전에, React가 페이지에서 요구되는 정보를 모두 가지고 있어야 하기 때문에 생성된 제약이다.

<br/>
<hr/>
<br/>

## Fetching Data at Request Time
**Static Generation**은 빌드 타임에 한 번만 작동했다.
결국, 요청 시에 정보가 필요하다면 **Server-side Rendering**을 사용할 수 .
**Server-side Rendering**을 이용하려면 **getStaticProps** 대신 **getServerSideProps**을 이용해야 한다.

<br/>

## Using getServerSideProps
**getServerSideProps**은 요청 시 호출되기 때문에, 파라미터로 요청한 context값을 받아야 한다.
이때, 서버가 모든 요청에 대해 결과를 계산해야 하고 추가 구성 없이는 CDN에 의해 결과를 캐시할 수 없기 때문에 **TTFB(Time to First Byte)**가 **getStaticProps**보다 느리다.
따라서 **getServerSideProps**은 요청 시에 데이터가 렌더링되는 경우에만 이용할 것을 권장한다.

<br/>

## Client-side Rendering(Static Generation with and without Data + Fetch Data on the Client-side)
미리 데이터를 렌더링할 필요가 **없다면**, **Client-side Rendering** 전략을 취할 수 있다.
* 외부 데이터가 필요 없는 페이지 부분을 정적으로 생성(사전 렌더링)한다.
* 페이지가 로드되면 JavaScript를 사용하여 클라이언트에서 외부 데이터를 가져오고 나머지 부분을 채운다.
이러한 접근 방식은, 일례로, 사용자 대시보드 페이지에 적합하다. 대시보드는 사용자별 개인 페이지이므로 SEO는 관련이 없으며 페이지를 미리 렌더링할 필요가 없다. 반면 데이터는 자주 업데이트되므로 요청 시간에 데이터를 가져와야 한다.

## SWR
Next.js 팀은 SWR(데이터 가져오기를 위한 React hook)를 만들었다. 클라이언트 측에서 데이터를 가져오는 경우 이를 적극 권장한다. 캐시부터 재검증, 포커스 추적, 간격 재설정 등을 모두 처리할 수 있다.
