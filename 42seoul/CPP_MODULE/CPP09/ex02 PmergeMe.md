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
- void fordJohnsonAlgorithmVector(size_t size);
- void pairSortVector(size_t size);
- void mergeInsertionSortVector(size_t low, size_t high);
- void fordJohnsonAlgorithmDeque(size_t size);
- void pairSortDeque(size_t size);
- void mergeInsertionSortDeque(size_t low, size_t high);
- void printInput();
- void printDeque();
- void printVector();
- void printTime();

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
## mergeInsertion flow
- pair == 1 일때 재귀를 탈출하며 mergeInsertion을 호출한다.
- main과 pending을 iterator로 구분할 것인가?
	- 데이터를 복사해서 가지고 main에 merge하고 해당 결과를 원본으로 복사한다?
- main의 값과 pending의 값을 짝지을 필요가 있다. -> pending을 통해 main의 어느 값과 쌍을 이루는지 알 수 있어야한다.
	- pending이 main에 merge되면 main의 인덱스가 변화하여 인덱스를 통한 계산은 불가능하다.
	- const하게 짝을 알 수 있어야한다.
	- pending은 pair를 가지는 vector?
	``` cpp
	std::vector<std::pair<std::vector<int>::iterator, std::vector<int>::iterator> > _pendings;
```

	1. main과 pending을 나눈다.
	2. pending의 첫번째 요소를 mainChain 맨 앞에 insert한다.
	3. pending을 main에 mergeInsertion한다.
		2. jacobsThal 수를 가져와 해당 pending 요소 부터 vector의 begin()까지 binarySearch로 main에 넣는다. -> end는 짝인 main의 값, start는 0
		3. begin()이 아니라 jacobs[n - 1] 까지 반복한다.
		4. 재귀적으로 다음 jacobsThal수를 가져와 begin()까지 반복한다.
		> pending의 요소를 main에 merge하면 pending의 인덱스가 변화하는데 어떻게 보정을 할것인가.
		> 		원본 vector를 직접적으로 수정을 할 것인가. 그렇다면 원본을 수정하면 iterator가 안전한가?
		> 		아니면, 다 종료되고 main의 iterator 배열을 토대로 원본을 수정할 수 있는가?
		> 		pair가 아니었던 요소들의 처리에 조심하자
	1. 재귀를 탈출하며 다음 size에서 mergeInsertion을 호출한다.
## binary_search flow
- iterator를 가지고 있는 vector인데 upper_dound()가 가능한가? -> upper_bound()에 사용자 정의 함수를 넣어서 할 수 있다.
	- iterator의 데이터를 비교하는 함수를 하나 만들어서 전달한다.

	1. upper_bound()를 이용해서 iterator를 리턴 받는다.
	2. 해당 위치에 pending first 값을 insert한다.
	3. pending에서 지운다?  -> 지우지 않고 mergeinsertion에서 n - 1 jacobs를 판단 할 수 있게 해준다.