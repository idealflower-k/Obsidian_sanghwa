#webserv 
# Setup routes with one or multiple of the following rules/configuration (routes wont be using regexp)
> 다음 규칙/구성 중 하나 이상을 사용하여 routes를 설정합니다(routes는 regexp를 사용하지 않음)

Execute CGI based on certain file extension (for example .php).
◦ Make it work with POST and GET methods.
◦ Make the route able to accept uploaded files and configure where they should be saved.
	∗ Do you wonder what a CGI is?
	∗ Because you won’t call the CGI directly, use the full path as PATH_INFO. ∗ Just remember that, for chunked request, your server needs to unchunk it, the CGI will expect EOF as end of the body. ∗ Same things for the output of the CGI. If no content_length is returned from the CGI, EOF will mark the end of the returned data. ∗ Your program should call the CGI with the file requested as first argument. ∗ The CGI should be run in the correct directory for relative path file access. ∗ Your server should work with one CGI (php-CGI, Python, and so forth). You must provide some configuration files and default basic files to test and demon- strate every feature works during evaluation.
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