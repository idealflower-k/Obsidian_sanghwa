# 명세
1. data.csv 파일을 열고 map의 형태로 parsing을 진행한다.
	1. 첫번째 라인의 forma을 확인한다. **date | value** 가 기본이지만 가운데 문자를 구분자로 잡아서 범용적인 format을 구현하면 좋을 듯
	2. 구분자로 잘 나누어져 있는지 검사
	3. date의 유효성 검사, 실제로 존재 가능한 날짜인지
	4. value의 유효성검사, 숫자(int, doucle)로만 이루어지고 양수만 가능
	5. map에 <struct, double>로 저장
