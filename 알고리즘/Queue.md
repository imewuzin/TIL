# Queue

: 스택과 마찬가지로 삽입과 삭제의 위치가 제한적인 자료 구조

- 큐의 뒤에서는 삽입만 하고, 큐의 앞에서는 삭제만 이루어지는 구조

- #### 선입선출 구조 (FIFO : First In First Out)
  
  - 큐에 삽입한 순서대로 원소가 저장되어, 가정 먼저 삽입된 원소는 가장 먼저 삭제된다
  
  - 머리(Front) : 저장된 원소 중 첫번째 원소(또는 삭제된 위치)
  
  - 꼬리(Rear) : 저장된 원소 중 마지막 원소

- 기본 연산
  
  | 연산              | 기능                                   |
  | --------------- | ------------------------------------ |
  | `enQueue(item)` | 큐의 뒤쪽(rear 다음)에 원소를 삽입. rear 증가      |
  | `deQueue()`     | 큐의 앞쪽(front)에서 원소를 삭제하고 반환. front 증가 |
  | `createQueue()` | 공백 상태의 큐 생성. front=rear=-1           |
  | `isEmpty()`     | 큐가 공백상태인지 확인                         |
  | `isFull()`      | 큐가 포화상태인지 확인                         |
  | `Qpeek()`       | 큐의 앞쪽(front)에서 원소 삭제없이 반환            |

---

## 선형 큐

: 1차원 배열을 이용한 큐

- 큐의 크기 = 배열의 크기

- `front` : 저장된 첫번째 원소의 인덱스

- `rear` : 저장된 마지막 원소의 인덱스

- 상태 표현
  
  - 초기 상태 : `front` = `rear` = -1
  
  - 공백 상태 : `front` == `rear`
  
  - 포화 상태 : `rear` = n-1 (n : 배열의 크기. n-1 : 배열의 마지막 인덱스)

- 구현
  
  - 초기 공백 큐 생성
    
    1. 크기 n인 1차원 배열 생성
    
    2. `front`와 `rear`를 -1로 초기화
  
  - 삽입 : `enQueue(item)`
    
    1. `rear`값 하나 증가시켜 새로운 원소 삽입할 자리 마련
    
    2. 그 인덱스에 해당하는 배열 원소 `Q[rear]`에 `item` 저장
    
    ```psuedocode
    def enQueue(item):
        global rear
        if isFull(): print("Queue_full")
        else:
            rear <- rear + 1;
            Q[rear] <- item
    ```
  
  - 삭제 : `deQueue()`
    
    1. `front`값 하나 증가시켜 큐에 남아있는 첫번째 원소 이동
    
    2. 새로운 첫번째 원소를 리턴함으로써 삭제와 동일한 기능함
    
    ```psuedocode
    deQueue():
        if(isEmpty()) then Queue_Empty();
        else{
            front <- front+1;
            return Q[front];
    }
    ```
  
  - 공백 상태 및 포화상태 검사: `isEmpty()`, `isFull()`
    
    - 공백상태 : `front == rear`
    
    - 포화상태 : `rear == n-1`
    
    ```psuedocode
    def isEmpty():
        return front == rear
    
    def isFull():
        return rear == len(Q)-1
    ```
  
  - 검색: `Qpeek()`
    
    1. 현재 `front`의 한자리 뒤(`front+1`)에 있는 원소, 즉 큐의 첫번째에 있는 원소 반환
    
    ```psuedocode
    def Qpeek():
        if isEmpty(): print("Queue_Empty")
        else: return Q[front+1]
    ```

- 문제점
  
  - 잘못된 포화상태 인식
    
    - 선형 큐 이용해 원소의 삽입과 삭제 계속할 경우, 배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구하고, `rear = n-1`인 상태(포화 상태)로 인식해 더 이상의 삽입을 수행하지 않게 됨

- 해결 방법
  
  - 매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시킴
    
    -> 원소 이동에 많은 시간이 소요되어 큐의 효율성이 급격히 떨어짐
  
  - 1차원 배열을 사용하되, 논리적으로는 배열의 처음과 끝이 연결되어 원형 형태의 큐를 이룬다고 가정하고 사용

---

## 원형 큐

- 초기 공백 상태 : `front = rear = 0`

- Index의 순환
  
  - `front`와 `rear`의 위치가 배열의 마지막 인덱스인 n-1을 가리킨 후, 그 다음에는 논리적 순환을 이루어 배열의 처음 인덱스인 0으로 이동해야 함
  
  - 이를 위해 나머지 연산자 `mod` 사용

- `front` 변수
  
  - 공백 상태와 포화 상태 구분을 쉽게 하기 위해 `front`가 있는 자리는 사용하지 않고 항상 빈자리로 둠

- 삽입 위치 및 삭제 위치
  
  |      | 삽입 위치                     | 삭제 위치                       |
  | ---- | ------------------------- | --------------------------- |
  | 선형 큐 | `rear = rear + 1`         | `front = front + 1`         |
  | 원형 큐 | `rear = (rear + 1) mod n` | `front = (front + 1) mod n` |

- 구현
  
  - 초기 공백 큐 생성
    
    1. 크기 n인 1차원 배열 생성
    
    2. `front`와 `rear`를 0으로 초기화
  
  - 삽입 : `enQueue(item)`
    
    1. `rear`값 조정해 새로운 원소 삽입할 자리 마련
    
    2. 그 인덱스에 해당하는 배열원소 `cQ[rear]`에 `item` 저장
    
    ```pseudocode
    def enQueue(item):
        global rear
        if isFull():
            print("Queue_Full")
        else:
            rear = (rear+1) % len(cQ)
            cQ[rear] = item
    ```
  
  - 삭제 : `deQueue()`, `delete()`
    
    1. `front` 값을 조정해 삭제할 자리 마련
    
    2. 새로운 `front`원소 리턴함으로써 삭제와 동일한 기능함
    
    ```psuedocode
    def deQueue():
        global front
        if isEmpty():
            print("Queue_Empty")
        else:
            front = (front+1) % len(cQ)
            return cQ[front]
    ```
    
    
  
  - 공백 상태 및 포화 상태 검사: `isEmpty()`, `isFull()`
    
    - 공백 상태 : `front == rear`
    
    - 포화 상태 : `(rear+1) mod n == front`
    
    ```pseudocode
    def isEmpty():
        return front == rear
    
    def isFull():
        return (rear+1) % len(cQ) == frong
    ```

---

## 연결 큐

: 단순 연결 리스트(Linked List)를 이용한 큐

- 구조
  
  - 큐의 원소 : 단순 연결 리스트의 노드
  
  - 큐의 원소 순서 : 노드의 연결 순서. 링크로 연결됨
  
  - `front` : 첫번째 노드를 가리키는 링크
  
  - `rear` : 마지막 노드를 가리키는 링크

- 초기 상태 : `front = rear = null`

- 공백 상태 : `front = rear = null`

---

## 덱 deque

: 양쪽 끝에서 빠르게 추가와 삭제를 할 수 있는 리스트류 컨테이너

- 연산
  
  - `append(x)` : 오른쪽에 x 추가
  
  - `popleft()` : 왼쪽에서 요소를 제거하고 반환. 요소가 없으면 `IndexError`
  
  ```python
  from collections import deque
  
  q = deque()
  q.append(1)  # enqueue()
  t = q.popleft()  # dequeue()
  ```

---

## 우선순위 큐 Priority Queue

: 우선순위를 가진 항목들을 저장하는 큐

- FIFO 순서가 아니라 우선순위가 높은 순서대로 먼저 나가게 됨

- 적용 분야 : 시뮬레이션 시스템, 네트워크 트래픽 제어, 운영체제의 테스크 스케줄링

- 배열을 이용한 우선순위 큐
  
  - 구현
    
    - 배열을 이용해 자료 저장
    
    - 원소를 삽입하는 과정에서 우선순위를 비교해 적절한 위치에 삽입
    
    - 가장 앞에 최고 우선순위의 원소가 위치하게 됨
  
  - 문제점
    
    - 배열을 사용하므로, 삽입이나 삭제 연산이 일어날 때 원소의 재배치 발생
    
    - 소요되는 시간이나 메모리 낭비가 큼

- 리스트를 이용한 우선순위 큐

---

### 큐의 활용 : 버퍼(Buffer)

: 데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역

- 버퍼링 : 버퍼를 활용하는 방식 또는 버퍼를 채우는 동작

- 자료 구조
  
  - 버퍼는 일반적으로 입출력 및 네트워크와 관련된 기능에서 이용됨
  
  - 순서대로 입력 / 출력 / 전달되어야 하므로, FIFO 방식의 자료구조인 큐가 활용됨

- 예시 - 키보드 버퍼
  
  1. 사용자 키보드 입력 : A P S (enter)
  
  2. 키보드 입력 버퍼 : A P S (enter)
  
  3. 키보드 입력 버퍼에 Enter 키 입력이 들어오면
  
  4. 프로그램 실행 영역 : (enter) S P A -> 연산
