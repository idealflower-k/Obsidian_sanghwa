#webserv
# 1. 목적
webserv 프로그램의 구현에 앞서 subject에 나와있는 요구사항과 제약사항들을 문서화하기 위해 작성된다.

| key | value |
| ---| --- |
| program name | webserv |
| Turn in files | Makefile, \*.{h, hpp}, \*.cpp, \*.tpp, \*.ipp, configuration files|
| Makefile | NAME, all, clean, fclean, re |
| Arguments | [A configuration file] |
| External functts. | Everything in C++ 98.<br>execve, dup, dup2, pipe, strerror, gai_strerror, errno, dup, dup2, fork, socketpair, htons, htonl, ntohs, ntohl, select, poll, epoll (epoll_create, epoll_ctl, epoll_wait), kqueue (kqueue, kevent), socket, accept, listen, send, recv, chdir bind, connect, getaddrinfo, freeaddrinfo, setsockopt, getsockname, getprotobyname, fcntl, close, read, write, waitpid, kill, signal, access, stat, opendir, readdir and closedir.|
| Libft authorized | n/a |
| Description | A HTTP server in c++ 98 |
- C++ 98로 HTTP 서버를 작성해야 한다.
- ./webserv [configuration file]
## 프로그램
### 1. 프로그램 인자
- 프로그램은 사용자에게 인자로 받은 설정 파일을 읽을 수 있어야 합니다.
- 설정 파일이 지정되지 않은 경우, 프로그램은 기본 경로에서 설정 파일을 찾아야 합니다.
### 2. 서버
- **비블로킹(non-blocking)**
- 서버는 언제나 block 되어서는 안되며, 필요한 경우 클라이언트를 적절히 종료될 수 있어야한다.
- 서버는 blocking 되어서는 안 되며, 클라이언트와 서버(리스닝 포함) 사이의 모든 입출력 작업에 대해 poll() (또는 그에 상응하는 함수)를 1회만 사용해야 한다.
- 설정 파일을 읽기 전에 poll()를 사용할 필요가 없다.
- 여러 포트에서 수신할 수 있어야 한다.
### 3. poll() 또는 그에 상응하는 함수
- 동시에 읽기와 쓰기를 확인해야 한다.
### 4. 매크로, define
- 예를 들어 FD_SET, FD_CLR, FD_ISSET, FD_ZERO를 사용할 수 있습니다.
### 5. HTTP 요청 및 응답 처리
- 서버에 대한 요청이 영원히 멈춰서는 안된다.
- 서버는 내가 선택한 웹 브라우저와 호환되어야 한다.
- HTTP 응답 상태 코드는 정확해야 한다.
- 서버는 기본 오류 페이지를 제공해야 한다.
- 완전히 정적인 웹사이트를 제공
- 클라이언트는 파일을 업로드할 수 있어야 한다.
- 서버는 최소한 GET, POST, DELETE 메서드를 지원해야 합니다.
- 서버는 설정 파일에 지정된 루트 디렉토리에서 파일을 찾아야 합니다.

## 설정 파일

### 1. 서버 설정
- **기능**: 사용자는 각 서버의 포트와 호스트를 선택할 수 있어야 합니다.

### 2. 서버 이름 설정
- **기능**: 사용자는 server_names를 설정하거나 비워 둘 수 있어야 합니다.
### 3. 기본 서버 설정
- **기능**: 첫 번째 서버 설정은 해당 호스트와 포트의 기본 서버로 지정되며, 다른 서버에 속하지 않는 모든 요청에 응답합니다.
### 4. 오류 페이지 설정
- **기능**: 사용자는 기본 오류 페이지를 설정할 수 있어야 합니다.
### 5. 클라이언트 바디 크기 제한
- **기능**: 사용자는 클라이언트 바디의 크기를 제한할 수 있어야 합니다.
- **입력**: 바디 크기 제한 값 (예: 2MB)
### 6. 루트 설정

- **기능**: 사용자는 다양한 규칙과 설정을 통해 루트를 설정할 수 있어야 합니다.
    - 허용된 HTTP 메서드 목록 정의
    - HTTP 리디렉션 정의
    - 파일 검색 위치 디렉토리 또는 파일 정의
    - 디렉토리 목록 표시 여부 설정
    - 요청이 디렉토리인 경우 디렉토리에 대한 기본 응답 파일 설정
    - CGI 실행 설정 (특정 파일 확장자 기반)
    - POST와 GET 방법 활성화
    - 루트가 업로드된 파일 수용 및 저장 위치 설정
### 7. CGI 처리

- **기능**: 서버는 CGI 처리를 올바르게 수행해야 합니다.
    - CGI 호출과 관련된 경로 설정 (PATH_INFO 사용)
    - 청크로된 요청의 경우, 서버가 이를 언청크해야 하며, CGI는 바디의 끝으로 EOF를 예상합니다.
    - CGI 출력 처리 (Content-Length 또는 EOF 기반)
    - 첫 번째 인수로 요청된 파일을 사용하여 올바른 디렉토리에서 CGI 실행
    - 하나의 CGI (예: php-CGI, Python)와 함께 작동
    - 테스트 및 데모를 위한 설정 파일 및 기본 파일 제공
- **입력**: CGI 호출에 필요한 요청 및 파일
- **출력**: CGI 처리 결과
- **예외처리**: 처리 오류 시 에러 메시지 출력

## 제약사항
### 1. 웹 서버 실행
- 다른 웹 서버를 execve로 실행할 수 없다.
### 2. 읽기 쓰기
- 읽거나 쓰기 작업을 poll()를 거치지 않고 진행해서는 안 된다.
- 읽기 또는 쓰기 작업 후 **errno**의 값을 확인하는 것은 엄격히 금지된다.
- poll()를 사용하지 않고 어떤 파일 디스크립터에서든 read/recv 또는 write/send를 시도하면, 점수는 **0점**이다!
- CGI 외의 다른 것에 **fork**를 사용할 수 없다.
## 보너스 파트
- 쿠키와 세션 관리 지원 (빠른 예시 준비).
- 여러 CGI 처리.



#### 연결 노트
[[42seoul/Webserv/subject/webserv_subject_note|subject 참고]]
[[CreateDocument/소프트웨어 요구사항 명세서 (SRS)]]
