# Reverse Polish Notation
## 기능  명세
### 1. 프로그램은 "" 로 감싸진 문자열 하나를 받는다.
	1. ac != 2 , error
	2. RPN(av[1]), 객체를 생성한다.
### 2. 문자열을 파싱한다.
	1. 문장열을 공백을 기준으로 분리하며 숫자만 stack에 넣는다. -> 0 ~ 9 만 가능, 숫자를 넣고 문자열이 끝나면 스택에 숫자는 1개만 존재해야한다.
	2. 숫자가 아닌 연산자이면 stack에서 숫자 2개를 pop해서 연산후에 넣는다. -> "+ - * /" 만 가능, 숫자가 2개 미만이면 "ERROR"
### 3. 계산을 한다.
	1.두개의 숫자와 연산자를 받아서 계산한 값을 리턴한다.
### 4. 결과를 출력한다.
	1. 문자열이 끝나고 스택에 숫자가 1개인 경우에만 출력한다.
	2. 스택에 숫자가 1개가 아니면 "ERROR"
	3. 종료
## 클래스 명세
### class RPN
#### 변수
- std::stack\<int\> \_storge
- std::string \_input
- int (\*operators[4]) (int, int) \_operators;
#### 멤버 함수
- 생성자, 소멸자, 복사생성자, 복사할당연산자
- calculator(); -> 메인 로직 함수
- std::string getOneData(); -> input 문자열에서 공백으로 구분해 요소하나를 리턴한다, input에서 해당 요소를 삭제하고 앞에 있는 공백을 삭제한다.
- void route(std::string str); -> str의 유형에 따라 setData(), calData()를 호출한다.
- bool validStr(std::string str); -> str의 유효성검사를 한다.
- void setData(int data); -> data를 storge에 push한다.
- void calData(std::string str); -> 연산자를 받아 스택에 있는 2개의 숫자를 연산해서 다시 push한다.
- std::stack\<int\> getTwoData(); -> storge에 있는 2개의 요소를 스택으로 가져온다, 2개 미만시 "ERROR"그리고 종료
- void printResult(); -> 최종 스택에 남은 요소를 출력한다, 2개이상이면 "ERROR"출력
- int calOp(std::string op); -> operators 배열에 있는 계산 함수 실행후 결과값 리턴
- int addition(int data1, int data2); -> 더하기
- int subtraction(int data1, int data2); -> 빼기
- int multiplication(int data1, int data2); -> 곱하기
- int division(int data1, int data2); -> 나누기, 0으로 나누면 "ERROR"
