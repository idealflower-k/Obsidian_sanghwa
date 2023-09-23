
config 파일을 받아서 실행
-> config FILE parsing

서버를 생성. 
-> server_fd read 이벤트 등록까지 listen (server init) 

클라이언트 접속. -> accept 과정 connection (server - client)
-  해당 클라이언트를 위한 소켓열고
- accept_fd (client_fd) 에대한 read 이벤트 등록

클라이언트 req 보냄. 
1. req parsing (검증)
2. 서버가 req에 대한 처리.(process)