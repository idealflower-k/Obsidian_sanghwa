# PmergeMe
## 기능 명세
	1. 양의 정수를 매개변수로 받는다. -> ./PmergeMe 1 2 3 4 5
	2. 병합-삽입 정렬 알고리즘을 사용해서 정렬한다. -> Ford-Johnson algorithm
	3. 에러가 발생하면 표준출력으로 에러를 출력한다.
	4. 2개이상의 서로 다른 컨테이너를 사용해서 정렬한다.
	5. 각 컨테이너에 대해 알고리즘을 구현한다.
	6. 표준 출력 지침
		1. 첫 번째 줄에는 정렬되기전의 정수들을 출력한다.
		2. 두 번째 줄에는 정렬후의 정수들을 출력한다.
		3. 세 번째 줄에는 첫 번째 컨테이너를 사용하여 정렬하는데 걸린 시간을 출력한다.
		4. 마지막 줄에는 두 번째 컨테이너를 사용하여 정렬하는데 걸린 시간을 출력한다.

## Ford-Johnson algorithm
	1. [n/2]개의 서로 다른 원소쌍들을 각 쌍별로 비교한다. (n이 홀수이면 마지막 원소는 남겨둔다.)
	2. 단계1 에서 판정한 더 큰 원소들을 [n/2]개를 병합 삽입으로 정렬한다.
	3. 주 사슬(b1, a1, a2, a3, ..., a[n/2]), 보조 사슬(b2, b3, b4, ..., b[n/2], b > [n/2])
	4. Jacobsthal sequence의 순서로 보조사슬을 메인 사슬에 병합 정렬한다.
## 클래스 명세
### class PmergeMe
#### 변수
- std::vector\<unsigned int\> \_vec
- std::deque\<unsignde int\> \_deq
- size_t \_sizeN
- std::time_t \_vecSortTime
- std::time_t \_deqSortTime
#### 멤버 함수
- PmergeMe(char \*\*av);
- void initData(char \*\*av);
- bool validData(std::string data);
- void strToUint(std::string str);
- void fordJohnsonAlgorithm();
- void pairSort(size_t size, size_t pairNum)
- void mergeInsertionSort(size_t low, size_t high);

## flow chart
	1. 매개변수로 받은 문자열들을 유효성 검사를 하면서 2개의 컨테이너에 넣는다.
	2. vector로 정렬을 시작하면서 시작시간을 저장한다.
	3. 정렬이 완료된 후 현재시간 - 시작시간을 저장한다.
	4. deque로 정렬을 똑같이 반복한다.
	5. 결과 출력후 종료
### 2. 정렬 flow chart
	1. size = 1, pair = n/size로 시작한다.
	2. pair로 묶인 요소들 끼리 큰 수가 앞으로 가게 정렬한다.
	3. size *= 2, pair = n/size로 진행된다.
	4. pair = 1이 되면 병합정렬을 하며 재귀를 탈출한다.