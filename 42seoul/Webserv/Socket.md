> 네트워크 프로그래밍에서 소켓은 주요 구성요소로, 소켓 API를 사용하여 클라이언트와 서버간의 통신이 이루어진다.
# 소켓의 생성과 설정
## 1. 소켓 생성 (**socket()** 함수)
- Application Layer : 애플리케이션은 socket() 함수를 호출하여 소켓을 생성한다.
- Kernel Layer : 커널은 소켓 디스크립터를 생성하고, 이 디스크립터는 추후 네트워크 연산에 사용된다.
## 2. 소켓 옵션 설정 (**setsocketopt()** 함수)
- Application Layer : 애플리케이션은 setsockopt() 함수를 사용하며 소켓의 다양한 행동을 설정한다. 소켓의 타임아웃 시간, 버퍼 크기 등을 설정한다.
- Kernel Layer : 커널은 설정된 옵션을 기반으로 소켓의 동작을 조절한다.
# 주소의 설정과 연결
## 1. 주소설정 (**bind()** 함수) (server)
- Application Layer : 서버는 bind() 함수를 호출하여 소켓에 로컬 주소 (IP, port)를 할당 한다.
- Kernel Layer : 커널은 지정된 주소에 소켓을 바인딩하고, 이 주소를 사용하여 네트워크에 통신을 수락한다.
## 2. 연락대기 (**listen()** 함수) (server)
- Application Layer : 서버는 listen() 함수를 호출하여 소켓을 수신 대기 상태로 설정한다.
- Kernel Layer : 커널은 소켓을 수신 대기 모드로 전환하며, 대기 중인 연결 요청읗 허용하고 큐에 저장한다.
## 3. 연결 수립 (**connect()** 함수) (client)
- Application Layer :
- Kernel Layer : 클라이언트는 connect()


