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
- socket