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
## *좋은 옵션들*
**SO_RCVBUF 와 SO_SNDBUF**
- 소켓의 수신 버퍼와 전송 버퍼의 크기를 조절할 수 있게 해준다.
- 버퍼 크기를 적절하게 조절하면 네트워크 통신의 성능을 향상시킬 수 있다.
```cpp
int buff_size = 4096;
setsockopt(socket_fd, SOL_SOCKET, SO_RCVBUF, &buff_size, sizeof(buff_size));
setsockopt(socket_fd, SOL_SOCKET, SO_SNDBUF, &buff_size, sizeof(buff_size));
```
**SO_KEEPALIVE**
- 네트워크 연결이 여전히 유효한지 확인하기 위해 keep-alive 메시지를 사용할 것이지를 설정한다.
```cpp
int optval = 1;
setsockopt(socket_fd, SOL_SOCKET, SO_KEEPALIVE, &optval, sizeof(optval));
```
**TCP_NODELAY**
- Nagle알고리즘을 비활성화 한다. [[WEB_note/socket/Nagle 알고리즘|Nagle 알고리즘]]
- 작은 패킷들이 빠르게 전송되도록 하여 지연시간을 줄일 수 있다.
- TCP 수준에서 설정된다.
```cpp
int optval = 1;
setsockopt(socket_fd, IPPROTO_TCP, TCP_NODELAY, &optval, sizeof(optval));
```
**SO_LINGER**
- 소켓이 close 함수가 호출될 때 어떻게 동작할지를 정의한다.
- 예를 들어, 데이터 전송이 완료되기 전에 소켓을 닫고 싶지 않은 경우에 사용할 수 있다.
```cpp
struct linger lin;
lin.l_onoff = 1; // 옵션의 활성화 여부
lin.l_linger = 10; // 소켓이 닫힐 때까지 대기할 시간(초) 설정
setsockopt(socket_fd, SOL_SOCKET, SO_LINGER, &lin, sizeof(lin));
```
- 소켓을 닫을 때, 아직 전송되지 않은 데이터가 있으면 지정된 시간(초)동안 그 데이터를 전송하려고 시도한다.
- 0초 설정 : 소켓을 닫을 때 연결을 강제로 리셋하고, 보류 중인 데이터를 모두 폐기한다.
## 서버 주소 바인딩 및 리스닝
```cpp
sockaddr_in server_addr;

std::memset(&server_addr, 0, sizeof(server_addr));

server_addr.sin_family = AF_INET;
server_addr.sin_addr.s_addr = INADDR_ANY;
server_addr.sin_port = htons(8080);

if (bind(listen_fd, (struct sockaddr*)&server_addr, sizeof(server_addr)) < 0) {
	std::cerr << "bind failed: " << strerror(errno) << "\n";
	close(listen_fd);
	return -1;
}

if (listen(listen_fd, 5) < 0) {
	std::cerr << "listen failed: " << strerror(errno) << "\n";
	close(listen_fd);
	return -1;
}
```
## kqueue 생성
>int kq = kqueue();
>struct kevent evSet;

## event set
>EV_SET(&evSet, server_fd, EVFILT_READ, EV_ADD | EV_ENABLE, 0, 0, NULL);