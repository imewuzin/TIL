# Jira 및 JQL

- 왜 Jira 사용?
  
  - Issue Tracking
  
  - Project Management - 대시보드 등 다양한 UI 제공
  
  - Agile
    
    - Scrum : 스프린트(기간) 제공
    
    - Kanban : 표st. 할 일 / 진행 중 / 완료 구분해 보기 쉬움
  
  - Scrum metting : 일정한 주기로 진행하는 짧은 회의
  
  - Backlog : 완료되지 않은 작업 항목들의 리스트나 목록
  
  - DevOps : Silo 현상 (내 곡식창고에만 쌓는 현상) 예방 가능
    
    - 반복적인 작업들을 Tool을 이용해 자동화
    
    - 팀원 모두가 알고있는 하나의 공유된 지표 제공
    
    - 장애나 이슈 있을 때 공유 가능
  
  - SRE (Site Reliability Engineering) : 소프트웨어 시스템의 안정성과 신뢰성을 유지하고 향상시키는 분야
  
  - AI(Atlassian Intelligence)
    
    - 자연어로 Jira에서 User Story을 빠르게 생성 및 글 내용의 어감 변경
    
    - Confluence 페이지와 Jira 이슈 요약
    
    - JQL이 아닌 자연어로 Jira 이슈를 간편하게 검색 가능

- 이슈 생성
  
  - 이슈 타입 (커스텀 가능)
    
    - 에픽 : 큰 단위의 작업. 여러 스프린트에 걸쳐 종료됨
    
    - 스토리 : 유저 스토리. 사용자 시나리오. 기능 단위(정하기 나름)
    
    - 작업 : 성능 개선 등. 에픽 혹은 스토리의 하위 작업
    
    - 버그 : 결함
  
  - 좌측 탭 '컴포넌트'에서 컴포넌트 생성 가능
  
  - 설명에서 AI 기능 이용 가능 (양식, 글 요약 등)
  
  - 레이블 : 하나의 해시태그라고 생각. 대소문자 구분
  
  - 상위항목 : 에픽 선택
  
  - 스프린트 : 선택 안 하면 백로그에 쌓임
  
  - 릴리스 (버전 관리)
    
    - 버그에서 많이 사용
  
  - Story Point : 난이도 표시

- ### JQL (Jira Query Language)
  
  : Jira Issue를 구조적으로 검색하기 위해 제공하는 언어
  
  - SQL (Standard Query Language)과 비슷한 문법
  
  - Jira의 각 필드들에 맞는 특수한 예약어들을 제공
  
  - 쌓인 Issue들을 재가공해 유의미한 데이터를 도출해내는데 활용(Gadget, Agile Board 등)
  
  - 종류
    
    - Basic Query : 쇼핑몰처럼 필터
    
    - Advanced Query
  
  - JQL Operators
    
    - `=`, `!=`, `>`, `>=`
    
    - `in`, `not in`
    
    - `~`(contains), `!~` (not contains) : 포함 여부
    
    - `is empty`, `is not empty`, `is null`, `is not null`
  
  - JQL Keywords
    
    - `AND`, `OR`, `NOT`, `EMPTY`, `NULL`, `ORDER BY`
  
  - JQL Dates
    
    - `current` : 오늘
    
    - `1d` : 내일
    
    - `-1d` : 어제
    
    - `1w` : 다음주 오늘요일
    
    - `-1w` : 지난주 오늘요일
  
  - JQL Functions
    
    - `endOfDay()` (0시), `startOfDay()` (24시)
    
    - `endOfWeek()` (Saturday), `startOfWeek()` (Sunday)
      
      - 월요일  표시 : `startOfWeek(+1d)`
    
    - `endOfMonth()`, `startOfMonth()`, `endOfYear()`, `startOfYear()`
    
    - `currentUser()` (지금 로그인한 사용자)
  
  - AI 이용해 자연어로도 검색 가능
  
  - JQL 활용 예시
    
    - 필터 저장 - 조회자에 공유할 사람 선택 - 편집기 - 저장 : 필터 저장됨
    
    - 상단 대시보드 - 대시보드 만들기 - 조회자,편집기 선택
      
      - 가젯 추가로 커스텀
        
        - 결과 필터 : 필터 설정해 볼 수 있음
        
        - 파이 차트 : 필터/프로젝트 선택해 생성
        
        - 히트맵 : 필터/프로젝트 선택해 생성. 글자 크기로 점유율 확인

- ### 보드
  
  - 기본 생성된 보드는 프로젝트 전체에 관한 보드
  
  - 보드 만들기로 기존 프로젝트/필터에 따라 보드 생성 가능

- 현업에서의 Jira 사용
  
  - Issue Tracking : Jira
  
  - Repo.Hosting : Bitbucket, gerrit, GitHub, GitLab
  
  - Code Review : Crucible, gerrit, GitHub, GitLab
  
  - Build & Deploy : Bamboo, Jenkins, circleci
  
  - Knowledge Base : Confluence

- GitLab과 Jira를 엮을 수 있음

- Jira의 플러그인 다양 - structure 플러그인 사용하면 폴더처럼 사용 가능

---

### Jira 용어 정리

- 일의 단위
  
  - 에픽
    
    : 완료하기까지 여러 번의 스프린트가 요구되는 큰 업무 단위
    
    - ex) 사용자 계정 및 인증 시스템 구현, 관리자 대시보드 개발, API 통합 및 외부서비스 연동
  
  - 스토리
    
    : 사용자 관점에서의 요구사항. 또는 구체적인 기능을 나타내는 작업 단위
    
    - ex) 로그인 기능, 상품 목록 조회 기능
  
  - 태스크
    
    : 스토리를 완료하기 위해 개발자가 작업해야 하는 단위 작업
    
    - ex) 토큰 갱신 로직, Oauth 연동, 검색 및 필터 기능 구현

- 백로그
  
  : 완료되지 않은 작업 항목들의 목록
  
  - 언제든지 이슈를 작성해서 올리면 됨 (상시 추가 가능)
  
  - **우선순위**로 정렬되어 있어야 함 (Product Owner의 역할)
  
  - 스토리 포인트 : 작업량 표시

- 스프린트 로그
  
  : 특정 스프린트 동안 수행하기로 약속한 작업 항목들의 목록
  
  - 팀의 작업 역량을 고려해 백로그에서 우선순위가 높은 항목들을 차례대로 스프린트 로그로 이동
  
  - 권장 : 월요일 아침 Planning meeting에서 팀내 합의 후 스프린트 로그 작성

- 스크럼 보드
  
  : 스프린트 동안 작업 항목의 진행 상황을 시각적으로 보여주는 보드
  
  - 사용 목적 : 팀의 진행상황을 누구나 투명하게 볼 수 있도록 공유
  
  - 진행중 컬럼 : 인당 1개의 작업 상황만 존재
  
  - 할 일 -> 진행 중 -> 리뷰 -> 완료 순서
