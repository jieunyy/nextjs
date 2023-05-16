## Creating API Routes
API 경로를 통해 Next.js 앱 내에 API 엔드포인트를 생성할 수 있다. 페이지/api 디렉토리 내에 함수를 생성하여 이를 수행할 수 있으며, 이는 서버리스 기능(Lamdas라고도 함)으로 배포된다.
<br/>
<br/>
## Creating a simple API endpoint

```js:hello.js
export default function handler(req, res) {
  res.status(200).json({ text: 'Hello' });
}
```
* req는 **http의 수신 메시지 인스턴스**이자 사전 형성된 미들웨어이다.
* res는 **http의 서버 응답 인스턴스**이며 일부 도우미 기능을 제공한다. <br/><br/>
## Do Not Fetch an API Route from getStaticProps or getStaticPaths
* **getStaticProps 또는 getStaticPaths에서 API 경로를 가져오면 안 된다**. 대신 getStaticProps 또는 getStaticPaths에 **서버 측 코드를 직접 작성**해야 한다(또는 도우미 함수 호출).

이는 getStaticProps 및 getStaticPaths는 서버 측에서만 실행되며 클라이언트 측에서는 실행되지 않기 때문이다. 또한 이러한 기능은 브라우저용 JS 번들에 포함되지 않는다. 따라서 직접 데이터베이스 쿼리와 같은 코드를 브라우저로 보내지 않고 작성해야 한다. <br/><br/>
## A Good Use Case: Handling Form Input
