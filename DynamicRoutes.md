# Dynamic Routes
https://nextjs.org/learn/basics/dynamic-routes

## Page Path Depends on External Data
각 페이지 경로가 외부 데이터에 의존하는 경우, Next.js를 사용하면 외부 데이터에 의존하는 경로가 있는 페이지를 정적으로 생성할 수 있다. 이는 동적 URL을 활성화한다.
<br/>
## Overview of the Steps
/posts/<id>에 해당하는 동적 URL을 활성화하기 위해서는,
1. **[id].js** 페이지를 만든다. [로 시작하고 ]로 끝나는 페이지는 Next.js의 동적 경로이다.
2. 이 페이지에서 **getStaticPaths**라는 비동기 함수를 내보낸다. 함수에서는 id에 대한 가능한 값 목록을 반환해야 한다. 
* *이때, 반환된 목록은 단순히 문자열 배열이 아니라 개체 배열이어야 한다. 파일 이름에 [id]를 사용하고 있기 때문에 각 개체에는 매개 변수 키가 있고 ID 키가 있는 개체가 포함되어 있어야 한다. 그렇지 않으면 getStaticPaths가 실패하게 되기 때문이.*
3. 마지막으로, **getStaticProps를 다시 구현**한다.

## Quick Review: 
  How does params.id from getStaticProps({ params }) know the key is named id?
  > The value from the file name

