# bitcoinExchange
## 기능명세
### 1. data.csv 파일을 열고 map의 형태로 parsing을 진행한다.
1. 첫번째 라인의 forma을 확인한다. **date ,exchange_rate** 가 기본이지만 가운데 문자를 구분자로 잡아서 범용적인 format을 구현하면 좋을 듯
2. 구분자로 잘 나누어져 있는지 검사
3. date의 유효성 검사, 실제로 존재 가능한 날짜인지
4. value의 유효성검사, 숫자(int, doucle)로만 이루어지고 양수만 가능
5. map에 <std::string, double>로 저장
### 2. inputFile을 열고 한줄씩 getline을 해서 result를 만들어 낸다.
1. 첫 줄을 읽어서 format을 확인한다. **date | value** 가 기본이다.
2. date의 유효성 검사
3. value의 유효성 검사, 0 ~ 1000사이
4. date 값으로 lower_bound()한 iterator의 -- 한 값 의 rate를 사용하여 곱해준다.
5. result 문자열을 만들어서 출력한다. 에러 발생시 에러 문자열을 출력하고 다음 getline실행
6. *개행* 에 유의하여 진행한다.
### 3. 프로그램을 종료한다.

## 클래스 및 함수 명세
### 1. Class BitcoinExchange
#### 변수
1. map<std::string, double> \_data
2. std::string \_inputFile
#### 멤버함수
1. 생성자, 복사생성자, 복사할당연산자, 소멸자
2. setData() -> data를 읽어 map에 할당한다.
3. vaildFormat() -> 첫라인에 들어온 format에 맞는지 확인한다.
4. vaildDate, vaildRate -> date검사, rate검사
5. vaildValue -> input Value 검사
6. multivalue() -> rate와 value 계산
7. printResult -> 결과 문자열 출력
8. convert() -> 전체 로직 함수