# sockaddr_in
- 소켓 프로그래밍에서 주로 사용되는 구조체
- 인터넷 주소를 저장하는데 사용된다.
```cpp
struct sockaddr_in {
	__uint8_t sin_len;
	sa_family_t sin_family;  // 주소 패밀리 (일반거으로 AF_INET)
	in_port_t sin_port;      // 포트 번호 (네트워크 바이트 순서)
	struct in_addr sin_addr; // IPv4 주소 (네트워크 바이트 순서) 
	char sin_zero[8];        // 0으로 채워져야 함 (페딩)
};
```
- sin_family : 주소 패밀리를 지정한다.
- sin_port : 네트워크 바이트 순서로 포트 번호를 지정한다. **htons()** 함수를 사용하여 호스트 바이트 순서에 네트워크 바이트 순서로 변환할 수 있다.
- sin_addr : IPv4주소를 저장하는 구조체, 주로 **inet_addr()**, **inet_pton()** 함수를 사용하여 주소를 설정
```cpp
struct in_addr {
	unsigned long s_addr;
}
```
- sin_zero : 패딩 목적으로 사용, **sockaddr** 구조체와 호환성을 유지하기 위해 0으로 채워져야 한다.
## sockaddr 와의 호환성
- sockaddr은 일반적인 소켓 주소 구조체로, sockaddr_in 과 유사한 역할을 하지만, 주소 패밀리에 따라 유연하게 사용할 수 있다.
```cpp
struct sockaddr {
	unsigned short sa_family; // 주소 패밀리
	char sa_data[14];         // 주소와 포트 정보를 담는 14바이트 필드
}
```
- sockaddr_in 구조체는 IPv4 주소를 명확하게 저장할 수 있게 해주지만, 때로는 sockaddr 구조체로 타입 캐스팅하여 함수 인자로 전잘해야 한다. ( **ex : bind(), accept()** )
- 소켓 API는 다양한 주소 패밀리와 프로토콜을 지원할 수 있어야 한다.
- sockaddr 구조체를 사용하여 일반적인 인터페이스를 제공하는 방법으로 해결된다.
- 이 구조체를 사용하면, IPv4, IPv6등 다른 주소 패밀리도 처리할 수 있다.
- sockaddr_in 구조체는 IPv4 주소를 처리하는데 특화된 구조체이다. 그러나, 소켓함수가 sockaddr 포인터를 인자로 받도록 설계되었기 때문에 sockaddr 구조체로 타입 캐스팅해야한다.

### sockaddr_in 을 사용하는 이유
- 특정 주소 패밀리와 관련된 데이터를 명확하게 표현하고 관리하기 위해
- sockaddr 의 sa_data 필드는 포트와 IP주소를 포함하여 모든 것을 담을 수 있지만, 형식이 정해져 있지 않다. 이것은 원시 데이터를 저장하기 위한 것이다.
- 대부분의 소켓 프로그래밍에서는 특정 주소 패밀리에 맞는 구조체를 사용하여 주소를 설정하고, 그 후에 sockaddr로 타입 캐스팅하여 필요한 함수에 전달한다.