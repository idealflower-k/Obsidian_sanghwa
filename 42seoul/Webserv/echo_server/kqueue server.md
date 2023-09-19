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

## 소켓 옵션 설정
>setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));

- SOL_SOCKET : 옵션의 수준을 나타낸다. SOL_SOCKET은 일반적인 소켓 옵션을 설정한다.
- SO_REUSEADDR : 소켓 주소 재사용을 가능하게 한다.
- &opt : 옵션의 값에 대한 포인터, opt = 1 로 설정되어 있으므로, 옵션을 활성화하겠다는 의미
- sizeof(opt) : 옵션 값의 크기
**SO_REUSEADDR 옵션**
- 서버가 재시작된 경우에도 이전에 사용되었던 주소를 재사용할 수 있게 한다.
- 일반적으로, 서버가 종료되면, 커널은 일정 시간동안 해당 주소를 "TIME_WAIT"상태로 유지한다.
- TIME_WAIT 상태의 주소를 재사용할 수 있다. 서버가 빠르게 재시작될 수 있게 해준다.
- **재사용 주소** : 소켓이 바인딩하는 IP주소와 포트의 조합을 의미한다.
**좋은 옵션들**

