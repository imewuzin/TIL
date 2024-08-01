# 배열 Array

: 일정한 자료형의 변수들을 하나의 이름으로 열거하여 사용하는 자료구조

- 배열을 사용하면 하나의 선언을 통해서 둘 이상의 변수 선언 가능

## 1차원 배열

- 선언
  
  - 별도의 선언 방법이 없으면 변수에 처음 값을 할당할 때 생성
  
  - 이름 : 프로그램에서 사용할 배열의 이름
  
  > ```python
  > Arr = list()
  > Arr = []
  > Arr = [1, 2, 3]
  > Arr = [0] * 10
  > ```

- 접근
  
  > ```python
  > Arr[0] = 10  # 배열 Arr의 0번 원소에 10을 저장하라
  > ```

- 입력받은 정수를 1차원 배열에 저장하는 법
  
  > ```python
  > # 첫 줄에 양수의 개수 N이 주어진다.
  > # 다음 줄에 빈칸으로 구분된 N개의 양수 Ai가 주어진다.
  > N = int(input())
  > arr = list(map(int, input().split()))
  > ```

---

## 순열 Permutation

: 서로 다른 것들 중 몇 개를 뽑아서 한 줄로 나열하는 것

- `nPr` : 서로 다른 n개 중 r개를 택하는 순열
  
  - `nPr = n * (n-1) * (n-2) * ... * (n-r-1)`

- `nPn = n!` : Factorial
  
  - `n! = n * (n-1) * (n-2) * ... * 2* 1`

---

## 2차원 배열

: 1차원 List를 묶어놓은 List

- 2차원 이상의 다차원 List는 차원에 따라 Index 선언

- 선언
  
  - 세로 길이(행의 개수), 가로 길이(열의 개수)를 필요로 함
    
    > ```python
    > arr = [[0, 1, 2, 3], [4, 5, 6, 7]]
    > """
    > | 0 | 1 | 2 | 3 |
    > | 4 | 5 | 6 | 7 |
    > """
    > 
    > 
    > arr = list(map(int, input().split())) for _ in range(N)]
    > arr = list(map(int, input())) for _ inrange(N)]
    > ```

- : n X m 배열의 n*m개의 모든 원소를 빠짐없이 조사하는 방법
  
  - 행 우선 순회
    
    > ```python
    > # i 행의 좌표
    > # j 열의 좌표
    > for i in range(n):
    >   for j in range(m):
    >     f(array[i][j]) # 필요한 연산 수행
    > ```
  
  - 열 우선 순회
    
    > ```python
    > # i 행의 좌표
    > # j 열의 좌표
    > for j in range(m):
    >   for i in range(n):
    >     f(array[i][j]) # 필요한 연산 수행
    > ```
  
  - 지그재그 순회
    
    > ```python
    > # i 행의 좌표
    > # j 열의 좌표
    > for i in range(n):
    >   for j in range(m):
    >     f(array[i][j + (m-1-2*j) * (1%2)])
    > ```

- 델타를 이용한 2차 배열 탐색
  
  : 2차 배열의 한 좌표에서 4방향의 인접 배열 요소를 탐색하는 방법
  
  > ```pseudocode
  > arr[0...N-1][0...N-1]  # N*N 배열
  > di[] <- [0, 1, 0, -1]  # 방향별로 더할 값
  > dj[] <- [1, 0, -1, 0]
  > for i : 0 -> N-1
  >   for j : 0 -> N-1
  >     for k in range(4):
  >       ni <- i + di[k]
  >       nj <- j + dj[k]
  >       if 0 <= ni < N and 0 <= nj < N  # 유효한 인덱스면
  >         f(arr[ni][nj])
  > ```

- 전치 행렬
  
  | 1   | 2   | 3   |     | 1   | 4   | 7   |
  | --- | --- | --- | --- | --- | --- | --- |
  | 4   | 5   | 6   | ->  | 2   | 5   | 8   |
  | 7   | 8   | 9   |     | 3   | 6   | 9   |
  
  > ```python
  > # i : 행의 좌표, len(arr)
  > # j : 열의 좌표, len(arr[0])
  > 
  > arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]  # 3*3 행렬
  > 
  > for i in range(3):
  >   for j in range(3):
  >     if i < j:
  >       arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
  > ```

- i, j의 크기에 따라 접근하는 원소 비교
  
  ![2차원리스트](https://github.com/user-attachments/assets/eac3264f-39ff-4eca-8706-8203bcc33446)

- ### 비트 연산자
  
  - `&` : 비트 단위로 AND 연산
  
  - `|` : 비트 단위로 OR 연산
  
  - `<<` : 피연산자의 비트 열을 왼쪽으로 이동
  
  - `>>` : 피연산자의 비트 열을 오른쪽으로 이동
  
  - 예시 문제 - 부분집합의 합 문제
  
  > - 부분집합의 수
  >   
  >   : 집합의 원소가 n개일 때, 공집합을 포함한 부분 집합의 수는 2**n 개. 각 원소를 부분집합에 포함시키거나 포함시키지 않는 2가지 경우를 모든 원소에 적용한 경우의 수와 같다.
  > 
  > - `1 << n`: 2**n. 즉, 원소가 n개일 경우의 모든 부분집합의 수 의미
  > 
  > - `i & (1 << j)` : i의 j번째 비트가 1인지 아닌지 검사
  > 
  > ```python
  > bit = [0, 0, 0, 0]
  > for i in range(2):
  >   bit[0] = i               # 0번 원소
  >   for j in range(2):
  >     bit[1] = j             # 1번 원소
  >     for k in range(2):
  >       bit[2] = k           # 2번 원소
  >       for l in range(2):
  >         bit[3] = l         # 3번 원소
  >         print_subset(bit)  # 생성된 부분집합 출력
  > ```
  > 
  > ```python
  > arr = [3, 6, 7, 1, 5, 4]
  > n = len(arr)                    # n: 원소의 개수
  > 
  > for i in range(1<<n):           # 1<<n : 부분 집합의 개수
  >   for j in range(n):            # 원소의 수만큼 비트를 비교
  >     if i & (1<<j):              # i의 j번 비트가 1인 경우
  >       print(arr[j], end=', ')   # j원소 출력
  >   print()
  > print() 
  > ```

---
