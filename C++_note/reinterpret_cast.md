#CPP

# Explanation
## reinterpret_cast 는 static_cast와 달리. const_cast 처럼 컴파일시에 CPU 명령어로 변환되지 않는다.
> *reinterpret_cast* 가 실행 시간에 어떠한 실제 처리를 수행하지 않는다는 것을 의미한다. 즉, *reinterpret_cast* 는 메모리에 저장된 데이터의 바이트를 재해석하는 방법을 컴파일러에 지시하는 컴파일 시간의 연산이다.

1. **메모리 재해석** : *reinterpret_cast* 는 주어진 데이터의 메모리 표현을 변경하지 않는다. 대신, 해당 데이터를 다른 타입으로 해석하라고 컴파일러에 지시한다.
   예를 들어, *int* 포잍너