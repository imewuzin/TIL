# 알고리즘

: 유한한 단계를 통해 해결하기 위한 절차나 방법

- 컴퓨터 분야에서 알고리즘을 표현하는 방법
  
  - 의사코드 (슈도코드, Pseudocode)
    
    > ```pseudocode
    > CalcSum(n)
    >   sum <- 0
    >     for i : 1 -> n
    >       sum <- sum + i
    >     return sum;
    > ```
  
  - 순서도

- 알고리즘의 특성
  
  - 정확성 : 얼마나 정확하게 동작하는가
  
  - 작업량 : 얼마나 적은 연산으로 원하는 결과를 얻어내느낙
  
  - 메모리 사용량 : 얼마나 적은 메모리를 사용하는가
  
  - 단순성 : 얼마나 단순한가
  
  - 최적성 : 더이상 개선할 여지없이 최적화되었는가

## 시간 복잡도 Time Complexity

: 알고리즘의 작업량을 표현. 실제 걸리는 시간 측정.

- ### 빅-오(O) 표기법
  
  : 시간 복잡도 함수 중에서 가장 큰 영향력을 주는 n에 대한 항만을 표시
  
  - 계수(Coefficient)는 생략해 표시
  
  - ex) O(3n+2) = O(3n) = O(n)
  
  - ![bigo시간복잡도](https://github.com/user-attachments/assets/e0037bbd-6e95-4961-9e65-b06a9d7e1d92)

- 빅-오메가 표기법

- 빅-세타 표기법

ㄴ
