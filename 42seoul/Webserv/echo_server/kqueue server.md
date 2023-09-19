https://hyeonski.tistory.com/m/9
- 코드에 오류가 하나 있음
[[42seoul/Webserv/함수/kqueue|kqueue]]
- kevent는 ident와 filter를 식별자로 삼는다.
	- kevent의 중복 등록을 막고, 해당 이벤트 발생조건이 부합하지 않을 경우(fd가 close될 경우 등)에는 kqueue에서 삭제되어 최소한의 kevent만 남아있을 수 있도록 관리한다.
---
# 대략적인 흐름
1. kqueue()로 kqueue를 할당
2. 이벤트의 변화(등록, 삭제 등)를 kevent()에 전달
3. kevent()가 return한 처리 가능 event들을 받아 알맞게 처리

# 코드 흐름 / 분석
## 소켓 생성
>int server_socket = socket(AF_INET, SOCK_STREAM, 0);
- AF_INET : IPv4 사용
- SOCK_STREAM : TCP 사용
- 0 : protocol 지정 type에 의해 암시적으로 지정 되므로 0사용

