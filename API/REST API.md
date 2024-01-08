#REST #API #WEB 
[참고링크](https://masteringbackend.com/posts/api-design-best-practices)
# REST (Representational State Transfer)
- API의  구조를  결정한다.
- API 를 REST 디자인 규칙과 결합하면 RESTful API라고 한다.
## API가 RESTful로 간주되기 위한 기준
- 클라이언트-서버 아키텍처는 HTTP를 통해 관리된다.
- 클라이언트와 서버 간의 데이터 교환을 단순화하는 캐시된 데이터이다.
- 클라이언트는 하이퍼링크를 사용하여 수행할 수 있는 다른 모든 작업에 액세스할 수 있어야 한다.
- 클라이언트-서버 통신은 상태 비저장이다. 즉, GET 요청 사이에 클라이언트 정보가 유지되지 않으며 각 요청은 분리되어 관련이 없다.
- 클라이언트 메시지에는 클라이언트가 처리할 수 있는 충분한 데이터와 지침이 있다.
- 유형별 서버를 구성하는 다계층 시스템(보안, 로드벨런싱 담당)
## 모범사례
### REST API는 JSON으로 수락하고 응답해야 한다.
- JSON (JavaScript Object Notation)
``` json
Object {Name: Matt, ID: 183829}
```
### 엔드포인트에서 동사 대신 병사 사용
- RESTful API에서 리소스의 이름을 지정할 때 명사를 기반으로 하여 각 엔드포인트에서 각자가 수행하는 작업을 나타내는 것이 중요하다.
- 이러한 리소스에 대한 기본 CRUD 작업을 수행하는 데 사용되는 모든 HTTP 메서드는 이미 동사 형식이기 때문
``` HTTP
GET http://www.api.com/students
```
- 이미 GET 메서드는 우리가 학생 데이터 모음을 검색하고 있음을 알려주는 데 자테적으로 충분하다.
### 이름 컬렉션에 복수 명사 사용
- 단수 명사와 복수 명사 중에서 선택할 때는 복수 명사를 선택하는 것이 좋다.
``` HTTP
GET /users
```
- 클라이언트는 우리가 단지 한 명의 사용자가 아닌 사용자 모음 데이터를 다루고 있다는 것을 알 것이다.
### 오류 처리에 상태 코드 사용
- 적잘한 오류 처리를 통해 앱을 더욱 강력하게 만들어 더 나은 사용자 경험을 제공하고 생산성도 향상시킬 수 있다.
-  Informational : Communicates the state of the request. It ranges from 100-199.
- Success : Communicate the Success of the request. It ranges from 200-299.
- Redirection : Sends response about some additional action needed to complete a request. It ranges from 300-399.
- Client Error : Sends error response that has to do with the client. It ranges from 400-499.
- Server Error : Sends error response that has to do with the server. It ranges from 500-599.
- 200 — OK
- 404 — Not found
- 500 — Internal Server Error
- 201 — Created
- 204 — No Content
- 304 — Modified
- 400 — Bad Request
- 401 — Unauthorized
- 403 — Forbidden
- 501 — Not Implemented
### 리소스 중첩 사용
- 하위 리소스라고도 하는 이소스를 중첩하는 것은 엔드포인트 간의 계층적 관계를 유지하는 데 중요하며 다양한 엔드 포인트가 상호 연결되는 방식도 보여준다.
- 예를 들어 Medium과 같은 사이트는 플랫폼에서 다양한 작가의 스토리를 허용하며 이 경우 유효한 중첩을 만드는 것과 같은 엔드 포인트를 갖는다.
``` http
https://medium.com/me/stories/public
```
``` http
https://medium.com/posts/postId/comments
```
- 이는 가독성에 도움이 된다. 하지만 리소스 중첩을 3개 수준 이하로 제한하는 것도 모범 사례이다.
### 필터링, 정렬 및 페이지 매김을 사용하여 요청된 데이터 검색
- API 엔드포인트가 사용자 목록 반환과 같이 많은 데이터를 반환하는 경우 필터, 정렬 및 페이지 매김을 사용하는 것이 좋다.
- 이는 API 사용자가 전체 요청을 로드하는 대신 필요한 데이터를 검색하는 데 도움이 되며, 이로 인해 데이터베이스가 느려질 수 있다.
``` http
GET /users?sort_by=+email
// and
GET /users?sort_by=email

https://myportfolio.com/posts?tags=restapi
```
### 보안을 위해 SSL 사용
- 해커는 자동화된 스크립트를 사용하여 API 서버를 공격할 수 있다.
- SSL (Secure Sockets Layer)
- 인터넷 연결을 보호하고 클라이언트와 서버 간에 공유되는 민감한 데이터를 유지하는 데 사용되는 표준 기술로, 인터넷 해커가 전송된 정보를 읽거나 수정하는 것을 어렵게 만든다.
- 초보자이거나 단일 도메인 SSL을 원하는 경우 몇 분 내에 발급될 수 있고 가장 높은 암호화를 제공하는 번거롭지 않은  [**domain validation certificate**](https://www.cheapsslshop.com/domain-validation-certificates) (DV SSL 인증서)가 필요하다.
- 더욱 안전한 업데이트 버전을 원할 경우 SSL을 TLS(전송 계층 보안)로 업데이트할 수도 있다.
### 잘 컴파일된 API 문서
- REST API 디자인에서는 문서화가 중요하다.
- 사용자에게 API 디자인을 쉽게 안내할 수 있을 만큼 강력해야 한다.
- 문서에 포함되어야 하는 기타 관련 정보
	- API 의 관련 엔드포인트에 대한 정보
	- 엔드포인트에 대한 요청을 보여주는 예
	- 개별 메시지를 포함하여 상태 코드가 포함된 다양한 오류에 대한 정보
### 버전 관리 전략 사용
- 버전 관리 전략을 통해 클라이언트는 기존 REST API를 계속 사용하고 준비가 되면 애플리케이션을 최신 API로 마이그레이션할 수 있다.
#### REST API 버전을 올바르게 지정하려면 기본적으로 다음과 같은 네 가지 방법을 따라야 합니다.
1. URL 경로를 통한 버전 관리
	- URI 경로에 버전 번호를 포함시키는 작업이 포함된다.
2. 쿼리 매개변수를 통한 버전 관리
	- 버전 번호를 쿼리 매개변수로 포함하는 작업이 포함된다.
3. 사용자 정의 헤더를 통한 버전 관리
	- 사용자 정의 헤더 제공 및 버전 번호를 속성으로 첨부하는 작업이 포함된다.
4. 콘텐츠 협상을 통한 버전 관리
	- 전체 REST API의 버전을 관리하는 대신 콘텐츠 협상 접근 방식을 사용하면 단일 리소스 표현의 버전을 관리할 수 있다.

[RESTful API 캐싱 전략](https://masteringbackend.com/posts/caching-strategy-for-restful-api)
