#python
# ASGI & WSGI
- ASGI (Asynchronous Server Gateway Interface)
- WSGI(Web Server Gateway Interface)
- 파이썬 웹 애플리케이션과  웹 서버 간의  통통신신을  위한 인터페이스이다.
## ASGI (Asynchronous Server Gateway Interface)
1. ASGI 의 기능 : ASGI 는 WSGI의 확장으로, 비동기 프로그래밍을 지원한다. 동시에 여러 요청을 처리할 수 있어 성능과 확장성이 향상된다.
2. 장점 : 고동시성 시나리오에서 뛰어난 성능을 제공하며, 실시간 업데이트, 긴 폴링 기술을 필요로 하는 애플리케이션 개발에 용이하다.
3. 한계 : 구현이 더 복잡할 수 있으며, 