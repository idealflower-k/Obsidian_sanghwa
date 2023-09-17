#webserv 
# webserv
[[42seoul/Webserv/subject/webserv_subject.pdf|webserv.pdf]]
# 요구 사항 명세
## 일반 규칙 (General rules)
- Makefile 제출
- **c++ -Wall -Wextra -Werror** 플래그 사용해서 컴파일 수행
- **-std=c++98** 플래그 사용
- 모든 외부 라이브러리 및 boost 라이브러리는 금지
## 필수 부분 (Mandatory part)
| key | value |
| ---| --- |
| program name | webserv |
| Turn in files | Makefile, \*.{h, hpp}, \*.cpp, \*.tpp, \*.ipp, configuration files|
| Makefile | NAME, all, clean, fclean, re |
| Arguments | [A configuration file] |
| External functts. | Everything in C++ 98.<br>execve, dup, dup2, pipe, strerror, gai_strerror, errno, dup, dup2, fork, socketpair, htons, htonl, ntohs, ntohl, select, poll, epoll (epoll_create, epoll_ctl, epoll_wait), kqueue (kqueue, kevent), socket, accept, listen, send, recv, chdir bind, connect, getaddrinfo, freeaddrinfo, setsockopt, getsockname, getprotobyname, fcntl, close, read, write, waitpid, kill, signal, access, stat, opendir, readdir and closedir.|
| Libft authorized | n/a |
| Description | A HTTP server in c++ 98 |
- C++ 98로 [[HTTP (HyperText Transfer Protocol)|HTTP]] 서버를 작성해야 한다.
- ./webserv [configuration file]

>  RFC를 읽고 텔넷 및 NGINX를 사용하여 몇 가지 테스트를 수행한 후 수행하시오.
### [[42seoul/Webserv/subject/Webserv 요구 사항|요구사항]]
1. 프로그램은 인자로 설정 파일을 받아야 하며, 그렇지 않으면 기본 경로를 사용해야 합니다.
2. 다른 웹 서버를 execve로 실행할 수 없습니다.
3. 서버는 언제나 블록되어서는 안되며, 필요한 경우 클라이언트는 적절히 종료될 수 있어야 합니다.
4. 서버는 블로킹되어서는 안 되며, 클라이언트와 서버(리스닝 포함) 사이의 모든 입출력 작업에 대해 poll() (또는 그에 상응하는 함수)를 1회만 사용해야 합니다.
5. poll() (또는 그에 상응하는 함수)는 동시에 읽기와 쓰기를 확인해야 합니다.
6. 읽기나 쓰기 작업을 poll() (또는 그에 상응하는 함수)를 거치지 않고 진행해서는 안 됩니다.
7. 읽기 또는 쓰기 작업 후 errno의 값을 확인하는 것은 엄격히 금지됩니다.
8. 설정 파일을 읽기 전에 poll() (또는 그에 상응하는 함수)를 사용할 필요가 없습니다.

> 비블로킹 파일 디스크립터를 사용해야 하기 때문에, poll() (또는 그에 상응하는 함수) 없이 read/recv 또는 write/send 함수를 사용할 수 있으며, 이 경우 서버는 블로킹되지 않습니다. 하지만 이렇게 하면 시스템 자원을 더 많이 소모하게 됩니다. 따라서, poll() (또는 그에 상응하는 함수)를 사용하지 않고 어떤 파일 디스크립터에서든 read/recv 또는 write/send를 시도하면, 당신의 점수는 0점이 될 것입니다.

9. 모든 매크로와 define, 예를 들어 FD_SET, FD_CLR, FD_ISSET, FD_ZERO를 사용할 수 있습니다. (그것들이 무엇을 하고 어떻게 작동하는지 이해하는 것은 매우 유용합니다.)
10. 서버에 대한 요청이 영원히 멈춰서는 안 됩니다.
11. 서버는 당신이 선택한 웹 브라우저와 호환되어야 합니다.
12. NGINX가 HTTP 1.1을 준수하고 있으며, 헤더와 응답 행동을 비교하는 데 사용될 수 있다고 간주합니다.
13. 당신의 HTTP 응답 상태 코드는 정확해야 합니다.
14. 서버는 제공되지 않은 경우 기본 오류 페이지를 가져야 합니다.
15. CGI(예: PHP, Python 등) 외의 다른 것에 fork를 사용할 수 없습니다.
16. 완전히 정적인 웹사이트를 제공할 수 있어야 합니다.
17. 클라이언트는 파일을 업로드할 수 있어야 합니다.
18. 최소한 GET, POST, DELETE 메서드가 필요합니다.
19. 서버에 스트레스 테스트를 실시하세요. 서버는 어떤 경우에도 사용 가능해야 합니다.
20. 서버는 여러 포트에서 수신할 수 있어야 합니다. (설정 파일 참조).

> MacOS에서는 write() 함수가 다른 Unix OS와 동일한 방식으로 구현되어 있지 않기 때문에, fcntl()을 사용할 수 있습니다. 다른 Unix OS와 유사한 동작을 얻으려면, 파일 디스크립터를 비블로킹 모드에서 사용해야 합니다.
> 
> 그러나 fcntl()은 다음과 같이만 사용할 수 있습니다: fcntl(fd, F_SETFL, O_NONBLOCK, FD_CLOEXEC); 그 외의 다른 플래그는 사용이 금지됩니다.
### 설정 파일
- 각 '서버'의 포트와 호스트를 선택하세요.
- server_names를 설정하거나 설정하지 마세요.
- 호스트:포트에 대한 첫 번째 서버는 해당 호스트:포트의 기본값이 됩니다(즉, 다른 서버에 속하지 않는 모든 요청에 응답합니다).
- 기본 오류 페이지를 설정하세요.
- 클라이언트 바디 크기를 제한하세요.
- 다음 규칙/설정 중 하나 또는 여러 개로 루트를 설정하세요 (루트는 정규 표현식을 사용하지 않습니다):
    - 루트에 대해 허용된 HTTP 메서드 목록을 정의하세요.
    - HTTP 리디렉션을 정의하세요.
    - 파일이 검색될 위치의 디렉토리나 파일을 정의하세요 (예: url /kapouet가 /tmp/www로 루트 설정된 경우, url /kapouet/pouic/toto/pouet는 /tmp/www/pouic/toto/pouet 입니다).
    - 디렉토리 목록 표시를 켜거나 끄세요.
    - 요청이 디렉토리인 경우 응답할 기본 파일을 설정하세요.
    - 특정 파일 확장자 (예: .php)를 기반으로 CGI를 실행하세요.
    - POST와 GET 방법으로 작동하게 만드세요.
    - 루트가 업로드된 파일을 받아들일 수 있도록 하고, 저장될 위치를 설정하세요.
	    - CGI가 무엇인지 궁금하신가요?
	    - CGI를 직접 호출하지 않기 때문에, PATH_INFO로 전체 경로를 사용하세요.
	    - 청크로된 요청의 경우, 서버가 이를 언청크해야 하며, CGI는 바디의 끝으로 EOF를 예상합니다.
	    - CGI의 출력에 대해서도 마찬가지입니다. CGI에서 content_length가 반환되지 않으면, EOF가 반환된 데이터의 끝을 표시합니다.
	    - 프로그램은 첫 번째 인수로 요청된 파일을 사용하여 CGI를 호출해야 합니다.
	    - CGI는 상대 경로 파일 액세스를 위해 올바른 디렉토리에서 실행되어야 합니다.
	    - 서버는 하나의 CGI (php-CGI, Python 등)와 작동해야 합니다. 평가 중 모든 기능이 작동하는 것을 테스트하고 보여주기 위해 몇 가지 설정 파일과 기본 기본 파일을 제공해야 합니다.

이 설명은 서버 설정과 CGI 실행, 파일 업로드 등 서버 구성에 관한 상세한 요구사항과 권고사항을 제시합니다.

>당신이 어떤 동작에 대해 궁금한 점이 있다면, 당신의 프로그램 동작을 NGINX의 동작과 비교해야 합니다. 예를 들어, server_name이 어떻게 작동하는지 확인하세요. 우리는 당신에게 작은 테스터를 제공했습니다. 브라우저와 테스트가 모두 잘 작동한다면 이를 통과할 필요는 없지만, 몇몇 버그를 찾아내는데 도움이 될 수 있습니다.

>중요한 것은 회복력입니다. 당신의 서버는 결코 다운되어서는 안됩니다.

>단 하나의 프로그램으로만 테스트하지 마세요. Python이나 Golang 등 더 편리한 언어로 테스트를 작성하세요. 원한다면 C나 C++로도 가능합니다.

## 보너스 부분 (Bonus part)
다음은 추가로 추가할 수 있는 기능들입니다:

- 쿠키와 세션 관리 지원 (빠른 예시 준비).
- 여러 CGI 처리.

>여러 CGI. 보너스 파트는 필수 파트가 완벽하게 완료된 경우에만 평가됩니다. 완벽하다는 것은 필수 파트가 완전히 완료되었으며 장애 없이 작동한다는 것을 의미합니다. 만약 모든 필수 요구사항을 통과하지 못했다면, 보너스 파트는 전혀 평가되지 않을 것입니다.



#### 연결 노트
[[WEB_note/HTTP/HTTP (HyperText Transfer Protocol)]]
