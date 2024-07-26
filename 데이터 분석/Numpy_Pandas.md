# Numpy & Pandas

## Numpy 기본 함수

- `import numpy as np`

- 기존 배열을 numpy array 로 변형
  
  - `np.array()`
    
    : 배열 만들어줌
    
    > ```python
    > arr = [1, 2, 3, 4, 5]
    > np_arr = np.array(arr)
    > print(np_arr)  # [1 2 3 4 5]
    > ```
  
  - `type()` 함수를 통해 타입 확인
    
    > ```python
    > print(type(np_arr))  # <class 'numpy.ndarray'>
    > ```

- 기본 배열 생성
  
  - `np.arange()`
    
    > ```python
    > arr = np.arange(15)
    > print(arr)  # [ 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14]
    > ```
  
  - python의 `range`함수와 비슷하게 사용 가능
    
    > ```python
    > arr = np.arange(10, 30, 5)
    > print(arr)  # [10 15 20 25]
    > ```
  
  - 2차원 배열 생성
    : `reshape(x축 개수, y축 개수)`
    : `np.arange()함수로 배열 생성 후 `reshape()`를 통해 2차원 배열로 변형
    
    - 주의사항 : `reshape()`의 x, y축 곱이 배열의 개수와 맞지 않으면 에러 발생
      
      > ```python
      > arr = np.arange(15).reshape(3,5)
      > print(arr)
      > """
      > [[0 1 2 3 4]
      >  [5 6 7 8 9]
      >  [10 11 12 13 14]]
      > """
      > ```
    
    - 0으로만 이루어진 배열 생성
      : `np.zeros((x축 개수, y축 개수), dtype=데이터타입)`
      
      > ```python
      > arr = np.zeros((3, 4), dtype=np.int64)
      > print(arr)
      > """
      > [[0 0 0 0]
      >  [0 0 0 0]
      >  [0 0 0 0]]
      > """
      > ```
    
    - 1로만 이루어진 배열 생성
      : `np.ones((x축 개수, y축 개수), dtype=데이터타입)`
      
      > ```python
      > arr = np.ones((3, 4), dtype=np.int64)
      > print(arr)
      > """
      > [[1 1 1 1]
      >  [1 1 1 1]
      >  [1 1 1 1]]
      > """
      > ```
    
    - 특정 수로 이루어진 배열 생성
      : `np.full((x축 개수, y축 개수), 값)`
      
      > ```python
      > arr = np.full((3, 4), 0.1)
      > print(arr)
      > """
      > [[0.1 0.1 0.1 0.1]
      >  [0.1 0.1 0.1 0.1]
      >  [0.1 0.1 0.1 0.1]]
      > """
      > ```          
      > ```
    
    - 균일한 간격의 숫자 생성
      : `np.linspace(구간 시작점, 구간 끝점, 구간 내 숫자 개수)`
      
      - 그래프의 x축 그릴 때 많이 사용
      
      - `lin`은 선형 간격 값을 생성하는 것을 나타내며, 로그 간격 값을 생성하는 `logspace`와 대조됨
        
        > ```python
        > arr = np.linspace(-5, 5, 10)
        > print(arr)
        > """
        > [-5.   -3.88888889     -2.77777778    -1.66666667     -0.55555556
        >  0.55555556     1.66666667     2.77777778   3.88888889     5.    ]
        > """
        > ```          
        > ```
    
    - 랜덤한 겂 생성
      : `np.random.rand(개수)`, `np.random.rand(x축 개수, y축 개수)`
      
      > ```python
      > arr = np.random.rand(5)
      > print(arr)  # [0.61047631 0.18521645 0.59498664 0.90255379 0.43010154]
      > ```
    
    - 원하는 범위 내의 정수 값을 원하는 개수로 생성하기
      : `np.random.randint(범위, size=개수)`
      
      > ```python
      > arr = np.randon.randint(5, size=10)
      > print(arr)  # [1 4 1 3 2 3 3 4 2 2]
      > ```
  
  - 기본 함수
    
    - `.shape` : 배열 각 축(axis)의 크기
    - `.ndim` : 축의 개수(차원, Dimension)
    - `.dtype` : 각 요소(Element)의 타입
    - `.itemsize` : 각 요소(Element)의 타입의 bytes 크기
    - `.size` : 전체 요소(Element)의 개수
  
  - 인덱싱 & 슬라이싱
    : python의 기본 Indexing과 동일
    
    - ndarray 에서 차이점은 `[a, b]`로 접근 가능
      
      > ```python
      > print(arr[1][2])  # 7
      > print(arr[1, 2])  # 7
      > ```
    
    - 2차원 배열 Slicing
      
      > ```python
      > arr = [[0, 1, 2, 3, 4], [5, 6, 7, 8, 9], [10, 11, 12, 13, 14], [15, 16, 17, 18, 19], [20, 21, 22, 23, 24]]
      > # 2행부터 3열까지
      > print([arr[:3] for arr in arr[2:]]  # [[10, 11, 12], [15, 16, 17], [20, 21, 22]]
      > arr = np.arange(25).reshape(5, 5)
      > # 1번행 이상 / 2번행 미만, 2번열 미만
      > print(arr[1:3, :2])  # [[5 6] [10 11]]
      > ```
  
  - 배열 값 수정 & 복사
    
    - Index 접근 후 수정은 기본 배열과 동일
    - Numpy도 python 기반이기 때문에 얕은 복사(Shallow Copy) 발생
      - 깊은 복사 : '실제 값'을 새로운 메모리 공간에 복사하는 것
      - 얕은 복사 : '주소 값'을 복사
    - 원본 배열을 수정하지 않으려면 `np.copy()` 함수 사용

---

## Pandas 사용법

1. `csv` 파일을 읽는 방법
   
   - Python 기본 `file open` 함수 사용 -> numpy로 변형
   
   - numpy 의 `file read` 함수 사용
     
     : `np.loadtxt(구분자 = ',', 데이터 타입:string)`
     
     > ```python
     > def file_open_by_numpy():
     >     np_arr = np.loadtxt('data/test_data.CSV', delimiter=",", encoding='cp949', dtype=str)
     >     return np_arr
     > 
     > arr = file_open_by_numpy()
     > print(arr)  # 2차원 배열 형태로 Read
     > ```
   
   - `pd.read_csv('csv파일명', encoding-'cp949')`
     
     - `parse_dates` : 열 이름을 넣어주면 데이터 타입을 datatime64[ns]로 변환
       
       - `pd.to_datatime()` 함수로도 datatime64로 변환 가능
       
       - '2019-01-01' 같은 string 형태의 자료형을 datatime64로 바꿈
     
     - `usecols` : 여러 열 중 필요한 열만 가져옴
     
     - `nrows` : 파일의 처음  n개 행 읽음. 데이터 세트 매우 큰 경우 유용

2. Pandas DataFrame 생성
   
   - `pd.DataFrame(arr)` : 기본적인 데이터 프레임 생성 
   
   - `pd.DataFrame(arr, columns=columns)` : 컬럼명 지정하며 생성. 인덱스명도 지정 가능

3. 기본 함수
   
   - `.head(n)` : 처음 n개 행 표시
   
   - `.tail(n)` : 마지막 n개 행 표시
   
   - `.columns = [리스트]` : 헤더 이름 지정하기
   
   - `.dtypes` : 행별 데이터 타입 반환
   
   - `.info()` : DataFrame의 정보 출력
   
   - `.describe()` : 컬럼 별 요약 통계량 출력
     
     - unique : 열에 있는 고유한 개체 수
     
     - top : 가장 많이 사용된 데이터
     
     - freq : 가장 많이 사용된 값의 빈도 수
     
     - NaN : 숫자가 아님 의미. 통계에 대한 계산을 할 수 없다는 의미
   
   - **`df['인덱스명'].value_counts()` : 발생 횟수와 함께 열의 고유 값 반환**
     
     - `df['인덱스명'].value_counts(normalize=True)` : 분포를 비율로 확인 가능
   
   - **`df['인덱스명'].astype(데이터 타입)` : 데이터 타입 변환**
     
     - 여러 열의 데이터 타입 한 번에 변경하려면 딕셔너리 사용
   
   - `iloc()` : row 번호로 특정 데이터 조회
   
   - `.loc[n]` : index 명으로 특정 데이터 조회
     
     - 여러 행 조회 가능. ex) `df.loc[[2, 3]]`
     
     - 특정 구간 조회 가능. ex) `df.loc[1:4]`
     
     - 특정 행 제외하고 조회 가능. ex) `df.loc[df.index != 2]`
     
     - 특정 행 다른 방법으로 가져오기
       
       - True/False 배열을 통해 True인 행만 가져오기. 이 방법은 인덱스 개수와 동일한 True/False 배열을 사용해야 에러가 발생하지 않음
       
       - ex) `df.head(5).loc[[True, True, False, False, True]]` -> 0, 1, 4번 행만 출력됨
     
     - 특정 열 조회 가능. ex) `df.loc[:, '이름']`
       
       - Column 이름을 배열 형태로 입력하면 DataFrame 형태(2차원)가 리턴됨. ex) `df.loc[:, ['이름']]`
     
     - 특정 열 제외하고 조회 가능. ex) `df.loc[:, df.columns != '나이']`
     
     - 특정 범위의 열 가져오기 가능. ex) `df.loc[:, '나이':'직업]`
     
     - 특정 열 다른 방법으로 가져오기
       
       - True/False 배열을 통해 True인 행만 가져오기. 이 방법은 인덱스 개수와 동일한 True/False 배열을 사용해야 에러가 발생하지 않음
       
       - ex) `df.head(5).loc[:, [True, True, False, False, True]]` -> 0, 1, 4번 열만 출력됨
     
     - 행과 열 조회하기 가능. ex) `df.loc[2, '나이']`, `df.loc[[2, 5], ['이름', '직업']]`
     
     - 마지막 위치에 하나의 행 추가 가능. ex) `df.loc[len(df)] = [데이터, 데이터, ...]`
   
   - `.append(추가하고 싶은 데이터의 딕셔너리, ignore_index=True)`
     
     : 마지막 위치에 여러 행 추가
     
     - 딕셔너리 형태를 DataFrame에 추가하고 싶을 때 사용
     
     - `ignore_index=True`로 설정해야 인덱스가 기존 행의 뒷 번호로 잘 지정됨
     
     - 주의 : Pandas 버전 업그레이드 시 사라질 수 있는 함수 (`concat`으로 대체)
   
   - `pd.concat([DataFrame, Dataframe], ignore_index=True)` : 두 개의 DataFrame 합치기
   
   - `pd.concat(df.iloc[:N], 원하는 행, df.iloc[N:], ignore_index=True)` : N번째 위치에 새로운 DataFrame 넣기
   
   - `df[열 이름] = 원소 리스트` : 마지막 위치에 하나의 열 추가
   
   - `.insert(인덱스, 열이름, 원소 리스트)` : 중간 위치에 열 추가
   
   - `.duplicated().sum()` : 중복 데이터 조회
   
   - `.drop_duplicates(inplace=True)` : 중복 데이터 제거
   
   - `.dropna()` : 결측 데이터 제거
     
     - 원본 DataFrame 변경하고 싶으면 `inplace=True` 설정해주기
     
     - `tresh=n` : 결측값이 아닌 열이 n개 이상일 경우 삭제하지 않는다는 기준
   
   - `df.['인덱스명'].fillna('채울 데이터', inplace=True)` : 결측값을 다른 적절한 겂으로 채워줌
   
   - `.drop([인덱스명 or 인덱스 순서] or 열 조건, inplace=True)` : 행 삭제
   
   - `.isna()` , `.notna()`: 결측값 존재 여부 확인 (무한 값이나 띄워쓰기는 결측값으로 판단하지 않음)
