#REST #API #WEB 
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
