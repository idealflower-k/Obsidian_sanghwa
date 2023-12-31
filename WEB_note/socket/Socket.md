>**소켓(socket)** 은 네트워크애서 서로다른 시스템 간에 데이터 통신을 가능하게 하는 인터페이스이다.

# 역할과 목적
## 1. 데이터 통신
- 소켓은 네트워크 통신의 기본 단위로, 서로 다른 컴퓨터나 프로세스가에 데이터를 주고받을 수 있는 통로 역할을 한다.
## 2. 프로토콜 구현
- 소켓은 TCP, UDP와 같은 네트워크 프로토콜을 구현, 이를 통해 안전하고 효율적인 데이터 통신이 가능하다.
## 3. 분산 시스템과 네트워크 서비스 구현
- 소켓을 통해 클라이언트-서버 모델을 기반으로 한 분산 시스템과 가양한 네트워크 서비스 (웹 서버, 이메일 서버 등)을 구현할 수 있다.
## 4. 자원 최적화
- 소켓 프로그래밍을 통해 통신 과정을 세밀하게 제어할 수 있으며, 이를 통해 네트워크 자원을 효율적으로 사용할 수 있다.
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
- Application Layer : 클라이언트는 connect() 함수를 호츨하여 서버에 연결을 시도한다.
- Kernel Layer : 커널은 TCP 3-way handshake 를 통해 서버와의 연결을 수립한다.
## 4. 연결 수락 (**accept()** 함수) (server)
- Application Layer : 서버는 accept() 함수를 호출하여 클라이언트의 연결 요청을 수락한다.
- Kernel Layer : 커널은 연결 대기 큐에서 연결 요청을 꺼내와 새로운 소켓 디스크립터를 생성하고, 서버 애플리케이션에 반환하여 특정 클라이언트와의 통신을 가능하게 한다.
# 데이터 전송과 수신
## 1. 데이터 전송 (**send() / recv() or wirte() / read()** 함수)
- Aplication Layer : 애플리케이션은 send(), recv() 함수를 사용하여 데이터를 전송하거나 수신한다.
- Kernel Layer : 커널은 데이터 패킷을 생성하고, TCP/IP 스택을 통해 패킷을 캡슐화하여 네트워크로 전송하거나 수신한다. 데이터 전송 시, TCP 프로토콜의 경우 흐름 제어, 혼잡 제어 등의 메커니즘을 사용하여 데이터를 전송을 조절한다.
# 연결의 종료
## 1. 연결 종료 (**close() or shutdown()** 함수)
- Aplication Layer : 한쪽(또는 양쪽)이 close(), shutdown() 함수를 호출하여 연결을 종료한다.
- Kernel Layer : 커널은 TCp 4-way handshake를 사용하여 연결을 안전하게 종료하며, 소켓과 관련된 자원을 해제한다.
# 커널 내부의 역할
- **Buffer Management**: 커널은 소켓 버퍼를 관리하여 데이터 전송 및 수신을 처리한다. 버퍼 오버플로우나 언더플로우를 방지하기 위한 메커니즘이 있다.
- **Protocol Stack**: 커널 내부에는 네트워크 프로토콜 스택이 있으며, 이를 통해 데이터 패킷이 적절하게 처리되고, 라우팅되며, 전송된다.
- **Error Handling**: 네트워크 통신 중 발생할 수 있는 다양한 오류 (예: 연결 끊김, 데이터 손실 등)를 감지하고 복구한다.
- **Security and Access Control**: 커널은 보안 및 접근 제어 메커니즘을 통해 네트워크 통신을 보호한다.