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