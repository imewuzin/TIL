# 파일과 디렉터리

## 파일

: 보조 기억 장치에 저장된 관련 정보의 집합

- 구성 : 파일 구성 정보, 실행 정보, 부가 정보(=속성, 메타 데이터)

- 속성
  
  - 유형 : 운영체제가 인지하는 파일의 종류
  
  - 크기 : 파일의 현재 크기와 허용 가능한 최대 크기
  
  - 보호 : 어떤 사용자가 해당 파일을 읽고, 쓰고, 실행할 수 있는지
  
  - 생성 날짜 : 파일이 생성된 날짜
  
  - 마지막 접근 날짜 : 파일에 마지막으로 접근한 날짜
  
  - 마지가 수정 날짜 : 파일에 마지막으로 수정된 날짜
  
  - 생성자 : 파일을 생성한 사용자
  
  - 소유자 : 파일을 소유한 사용자
  
  - 위치 : 파일의 보조기억장치상의 현재 위치

- 파일 연산을 위한 시스템 호출
  
  : 파일 생성, 삭제, 열기, 닫기, 읽기, 쓰기 등

## 디렉터리

: 윈도우의 폴더

- 1단계 디렉터리 : 옛날 운영 체제의 디렉터리. 디렉토리 1개

- 트리 구조 디렉터리 : 여러 계층으로 파일 및 폴더 관리
  
  - 루트 디렉터리(`/`), 서브 디렉터리

- 같은 디렉터리에는 동일한 이름의 파일이 존재할 수 없지만, 서로 다른 디렉터리에는 동일한 이름의 파일이 존재할 수 있음

- 경로
  
  : 디렉터리를 이용해 파일과 디렉토리 위치, 나아가 이름까지 특정 지을 수 있는 정보
  
  1. 절대 경로 : 루트 디렉터리에서 자기 자신까지 이르는 고유한 경로
     
     - ex) `/home/a.py`
  
  2. 상대 경로 : 현재 디렉터리에서 자기 자신까지 이르는 경로

- 디렉터리 연산을 위한 시스템 호출
  
  : 디렉터리 생성, 삭제, 열기, 닫기, 읽기 등

- #### 디렉터리 엔트리
  
  - 많은 운영체제에서는 디렉터리를 그저 특별한 형태의 파일로 간주
  
  - 즉, 디렉터리는 그저 포함된 정보가 조금 특별한 파일
  
  - 파일의 내부에는 파일과 관련된 정보들이 있다면, 디렉터리 내부에는 해당 디렉터리에 담겨있는 대상과 관련된 정보들이 담겨있음 (보통 테이블 형태)
  
  - 각 엔트리(행)에 담기는 정보
    
    - 디렉터리에 포함된 대상의 이름
    
    - 그 대상이 보조기억장치 내에 저장된 위치(를 유추할 수 있는 정보)
  
  - cf) `.` : 현재 디렉터리, `..` : 상위 디렉터리

---

## 파일 시스템

: 파일과 디렉토리를 보조기억 장치에 저장하고 접근할 수 있게 하는 운영 체제의 내부 프로그램

- 기능
  
  - 여러 파일과 디렉토리를 보조 기억 장치에 할당하고 접근.
  - 다양한 종류의 파일 시스템 존재 (예: FAT, 유닉스).
  - 하나의 컴퓨터에서 여러 개의 파일 시스템 사용 가능.

- 용어
  
  - 파티셔닝
    
    : 저장 장치의 논리적 영역을 구획하는 작업
    
    - ex) 서랍에 물건을 정리하기 위한 칸막이 설치
    
    - 파티션이라는 논리적 단위 생성
    
    - 
  
  - 포매팅
    
    : 파일 시스템을 설정. 어떤 방식으로 파일을 관리할지 결정, 새로운 데이터를 쓸 준비하는 작업
    
    - 종류
      
      - 저수준 포매팅: 물리적인 포맷팅
      
      - 논리적 포매팅: 일반적으로 사용하는 포맷팅
    
    - 파일 시스템에는 여러 종류가 있고, 파티션마다 다른 파일 시스템 설정 가능
    
    - 포메팅까지 완료하여 파일 시스템 설정했다면, 파일과 디렉터리 생성 가능

- #### FAT 파일 시스템 (File Allocation Table)
  
  : 연결 할당 기반 파일 시스템. 연결 할당의 단점을 보완
  
  - 각 블록에 포함된 다음 블록 주소를 한데 모아 FAT 테이블로 관리
  
  - FAT 테이블은 각 블록의 다음 블록 주소를 저장하여 접근 속도 향상
  
  - FAT 8, FAT 12, FAT 16, FAT 32 등 여러 버전이 있음
  
  - 디렉터리 엔트리 : 파일 이름, 확장자, 속성, 예약 영역, 생성 시간, 마지막 접근 시간, 마지막 수정 시간, 시작 블록, 파일 크기
  
  - 파티션 구조
    
    - 예약 영역: 특정 데이터 저장
    
    - FAT 영역: 파일 할당 테이블
    
    - 루트 디렉토리: 최상위 디렉토리
    
    - 데이터 영역: 실제 파일 및 서브 디렉토리 저장

- #### 유닉스 파일 시스템
  
  : 색인 할당 기반 파일 시스템
  
  - **i-node**(색인 블록) : 파일 속성 및 블록 주소 저장 (최대 15개)
  
  - 파일 크기가 클 경우
    
    1. 블록 주소 중 12개에는 직접 블록 주소 저장
       
       - 직접 블록 : 파일 데이터가 저장된 블록
    
    2. 1번으로 충분하지 않다면 13번째 주소에 단일 간접 블록 주소 저장
       
       - 단일 간접 블록 : 파일 데이터를 저장한 블록 주소 저장
    
    3. 2번으로 충분하지 않다면 14번째 주소에 이중 간접 블록 주소 저장
       
       - 이중 간접 블록 : 단일 간접 블록 2개의 주소 저장
    
    4. 3번으로 충분하지 않다면 15번째 주소에 삼중 간접 블록 주소 저장
       
       - 삼중 간접 블록 : 이중 간접 블록 3개의 주소 저장
  
  - 디렉터리 엔트리 : i-node 번호, 파일 이름

### 파일 할당 방식

: 포매팅까지 끝난 하드 디스크에 파일 저장하기

- 블록 단위 저장
  
  : 운영 체제는 파일과 디렉토리를 블록 단위로 읽고 씀
  
  - 블록: 여러 개의 섹터로 묶인 단위
  - 파일이 저장될 때 여러 블록에 걸쳐 저장 가능
1. **연속 할당**
   
   : 파일이 연속적인 블록에 할당되는 방식
   
   - 연속된 파일에 접근하기 위해 파일의 첫 번째 블록 주소와 블록 단위의 길이만 알면 됨
   
   - 디렉터리 엔트리 : 파일 이름, 첫 번째 블록 주소, 블록 단위 길이
   
   - 간단하지만, 외부 단편화 발생 가능

2. **불연속 할당**
   
   : 파일이 여러 블록에 걸쳐 불연속적으로 할당됨
   
   - 하위 유형
     
     - **연결 할당**
       
       : 각 블록의 일부에 다음 블록의 주소를 저장해 각 블록이 다음 블록을 가리키는 형태로 할당
       
       - 파일을 이루는 데이터 블록을 연결 리스트로 관리
       
       - 파일이 여러 블록에 흩어져 저장되어도 무방
       
       - 디렉터리 엔트리 : 파일 이름, 첫 번째 블록 주소, 블록 단위 길이
       
       - 반드시 첫 번째 블록부터 하나씩 읽어들여야 함
       
       - 오류 발생 시 해당 블록 이후 블록은 접근 어려움
     
     - **색인 할당**
       
       : 파일의 모든 블록 주소를 색인 블록이라는 하나의 블록에 모아 관리하는 방식
       
       - 파일 내 임의의 위치에 접근하기 용이
       
       - 특정 파일이 여러 블록에 걸쳐 저장될 때, 색인 블록 하나만 읽으면 모든 블록에 접근 가능
       
       - 디렉터리 엔트리 : 파일 이름, 색인 블록 주소
