---
tags:
  - JIRA
  - Agile
---
[참고 url](https://medium.com/hgmin/devops-jira%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%ED%98%91%EC%97%85-4f4049a36a56)
### [[Agile/Scrum|Scrum]]
---
# [JIRA]([https://www.atlassian.com/ko/software/jira](https://www.atlassian.com/ko/software/jira))란
- Planning : user story 및 issue(일감)를 생성하고 sprint를 계획
- Tracking : 팀 업무의 우선순위를 정하고, 수행 상태 등 가시성 제공
- Release : 일감의 개발완료 등 최신 정보를 가지고 제품 출시 관리
- Report : 실시간 시각적 데이터를 기반으로 팀 효율을 향상
- 

## 1. 일감(Issue) 작성 및 상태 관리
### 1.1 일감(Issue) 종류
- **Epic** : 큰 단위의 업무 (여러 User Story, Task 등을 묶은 단위)
- **User Story** : 최종 고객에게 가치를 제공하는 기능
	- 작성 방법 : "I as *WHO* want to do *WHAT*, so that *WHY*"
	- TIP : User Story의 크기는 sprint내에 완료 가능한 단위로 분할 필요
	- ex) : 사용자 관리 개발
- **Task** : User Story외의 기술적, 관리적 업무
	- ex) : 설계, 서버 설치, 클라우드 도입등
- **Sub-Task** : Story, Task를 더 작은 단위로 나눈 업무
	- 즉, 모든 Sub-Task가 끝나야 해당 업무 종료
	- ex) : 사용자 관리(UI) 개발, 사용자관리(Service) 개발
>참고로, JIRA에서는 Story와 Task를 같은 레벨로 구분하지만, 일반적으로 Story를 더 작게 나눈것을 Task라고 정의하기도 한다.

### 1.2 일감(issue) 작성
1. Epic 생성
2. User Story 추가
	1. 관련된 Epic link 연결
3. Sub-task 생성

### 1.3 보고자 및 당담자 할당
- 보고자(Repoter) : 일감 업무내용을 작성하고 , 진행상황을 보고 받는자
- 담당자(Assignee) : 해당 일감을 수행하고, 일감상태를 현행화할 담당자
### 1.4 우선순위 조정
- 일반적으로 Product owner는 일감의 우선순위로 정렬(특히 담당자별 수행해야할 작업이 여러개라면)
### 1.5 일감 진행 상태 변경
- Drag&Drop으로 상태명의 컬럼간 이동
- 작업 요청자(기획자)가 검토 후 최종 완료 상태로 이동
	- 개발자가 완료하면 "리뷰 중" 상태로 변경시키고
	- 기획자는 검토하고 "완료" 상태로 변경한다.
### 1.6 댓글 작성
- **@** 로 태그가능
### 1.7 Watcher(지켜보기)
- JIRA는 기본적으로 보고자, 담당자에게만 일감 변경, 댓글 발생시 알림이 가며, 담당자를 1명만 선택 가능 따라서, 다른 사람에게도 알리는 기능 필요
1. 숫자(Watch하고 있는 사람수)를 클릭해서 다른 사람 추가
2. 본인이 관심있는 일감을 클릭해서 추가
### 1.8 일감 완료

