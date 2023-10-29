# Client의 연결종료가 발생하는 곳
1. 클라이언트의 read() == 0이 되고 write할 내용이 없는 상황
	1.1. 하지만, 앞서 파싱된 내용이 write이벤트에 등록 된 상태인지 판단 필요
		1.1.1. read하는 fd만 닫을 수 있는 함수가 허용되지 않음

2. 클라이언트의 마지막 활동시간이 keepalive_timeout을 초과한 경우
	2.1. kevent의 timer 이벤트를 통해 확인하는 이벤트handler등록 하면 될 듯?
	2.2. keepalive_timeout을 초(s)단위로 판단해도 되는 지 확인 필요

3. 이벤트 처리중 에러가 발생한 상황
	3.1 연결을 종료하는 함수를 호출할 수 있는 구조가 필요하다.
	3.2 앞선 이벤트에서 에러가 발생했을 때 다음 이벤트 처리에서 터지지 않는 처리가 필요

4. write를 다 하고 나서? -> keepalive로 요청을 다시 받을 수 있기 때문에 종료하지 않는다.

# 연결을 종료하기 위해 필요한 작업
1. Server가 가지고 있는 _clients map_ 에서 Client객체를 지운다
2. Client객체의 소멸자가 호출되며 close(fd);
3. 해당 fd에 대해서 이미 등록된 이벤트들이 예상치 못한 동작을 하지 못하게 해야한다.
	1. 이벤트에 공통으로 알 수 있는 상태 부여?
	2. Dispatcher를 활용해서 처리하면 안되는 fd리스트 만들기? -> 새로 할당 받아서 중복된 fd를 고려해야한다. -> 언제까지 해당 fd를 처리하면 안되는지 고려

# 연결 종료작업을 위해 class가 알고 있어야 하는 정보
1. Client 이벤트 관련 handler들
	1. 이벤트의 에러, read의 완료 -> Client의 disconnect가 가능해야한다.
	2. 해당 동작을 위해 Client를 가지고 있는 Server의 removeClient를 사용해야 한다.
	3. 추가로 자신과 같은 fd를 사용중인 이벤트들이 오작동 하지 않도록 한다.
2. ServerAcceptHandler에서 ICallback을 알고 있다.
	1. 해당 이벤트에서 Client가 생성되고 Client의 ReadHandler를 만들고 등록 한다.
3. ClientReadHandler 혹은 Client의 execute에서 ClientWriteHandler를 등록 할 것이다.
	1. Read에서 등록하면 ICallback 전달이 가능하다.
	2. execute에서 등록하면 Client도 ICallback을 알아야 한다?