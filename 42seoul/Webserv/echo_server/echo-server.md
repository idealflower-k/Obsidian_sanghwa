# Server Code
## 헤더 파일
```cpp
#include <iostream>
#include <string>
#include <cstring>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>
```

## 메인 함수
```cpp
int main() {
	int server_fd, client_fd;
	struct sockaddr_in server_addr, client_addr;
	char buffer[1024];
	int port = 8080;

```
- server_fd : 서버 소켓의 파일 디스크립터
- client_fd : 클라이언트 소켓의 파일 디스크립터
- server_addr, client_addr : 서버와 클라이언트의 주소 정보를 저장하는 구조체
- buffer : 클라이언트로부터 메시지를 받아 저장하는 버퍼
- port : 서버가 리스닝할 포트 번호
```cpp
	if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
		perror("Socket failed");
		exit(EXIT_FAILURE);
	}
```
- socket() : 소켓을 생성한다.
```cpp
server_addr.sin_family = AF_INET;
server_addr.sin_addr.s_addr = INADDR_ANY;
server_addr.sin_port = htons(port);

if (bind(server_fd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
    perror("Bind failed");
    close(server_fd);
    exit(EXIT_FAILURE);
}
```
- bind() : 서버 소켓을 지정된 IP 주소와 포트에 바인드한다.
- INADDR_ANY : 서버가 모든 네트워크 인터페이스에서 클라이언트 연결을 수락하도록 한다.
```cpp
if (listen(server_fd, 1) < 0) {
    perror("Listen failed");
    close(server_fd);
    exit(EXIT_FAILURE);
}
```
- listen() : 서버 소켓이 클라이언트 연결을 기다라도록 설정
- 1 : 최대 대기 중인 연결의 수
```cpp
int addrlen = sizeof(client_addr);

if ((client_fd = accept(server_fd, (struct sockaddr *)&client_addr, (socklen_t*)&addrlen)) < 0) {
    perror("Accept failed");
    close(server_fd);
    exit(EXIT_FAILURE);
}
```
- accept() : 클라이언트의 연결을 수락한다. 연결된 클라이언트의 소켓 디스크립터를 반환한다.
```cpp
while (true) {
    int read_val = read(client_fd, buffer, 1024);
    if (read_val == 0) {
        std::cout << "Client disconnected" << std::endl;
        break;
    }
    buffer[read_val] = '\0';
    std::cout << "Client: " << buffer << std::endl;

    send(client_fd, buffer, read_val, 0);
}
```
- 무한 루프 안에서 클라이언트로부터 메시지를 읽고, 동일한 메시지를 클라이언트에게 다시 보내서 에코한다.

---
## 연결 노트
- [[42seoul/Webserv/함수/함수 정리|함수 정리]]
