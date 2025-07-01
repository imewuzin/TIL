# 기본 문법

1. 데이터 타입
   
   - 기본형 : 미리 정해진 크기의 데이터 표현. 변수 자체에 값 저장
     
     ![](C:\Users\SSAFY\AppData\Roaming\marktext\images\2025-04-14-14-24-52-image.png)
   
   - 참조형 : 크기가 미리 정해질 수 없는 데이터 표현. 변수에는 실제 값을 참조할 수 있는 주소만 저장
     
     - String
       
       - `String str1 = "Hello";` 문자열 풀(찾아보다 없으면 추가. 있으면 그대로 가져다 씀)
       
       - `String str2 = new String("Hello");` Heap에 매번 새로 만듦
       
       - Text Block (multi line) : `""" ~~ """`
       
       - `var` 키워드를 통한 로컬 변수 선언 : 변수에 값 할당 시 타입 적용 (변수 선언 시 값 할당까지 진행되어야 함)

2. 조건문
   
   - `if - else if - else` : 저건은 `boolean`일 경우만 가능
   
   - `switch` : `int`, `String`, `byte`, `char`, `boolean` 등 가능. `float`, `double` 불가능
   
   - Switch expressions : 표현식 자체가 값을 반환
     
     - ```java
       int month = 3;
       int day = switch (month) {
           case 2 -> 29;
           case 4, 6, 9, 11 -> 30;
           default -> {
               yield 31;
           }
       };
       ```

3. 반복문
   
   - `for`문, `while`문, `do-while`문
   - `for-each` with Array : `for (int x : intArray) {}`

4. 배열
   
   - 최초 메모리 할당 이후 변경 불가. 개별 요소는 다른 값으로 변경 가능하나, 요소를 추가하거나 삭제할 수 없음
   
   - 생성 : `new data_type[length]`
     
     - ex) `new int[3]`, `points = new int[3]`(points는 메모리에 있는 배열을 가리키는 reference 타입 변수)
   
   - 초기화 : 생성과 동시에 저장 대상 자료현에 대한 기본값으로 default 초기화 진행
     
     - 생성과 동시
       
       ```java
       int [] b = new int [] {1, 3, 5, 6, 7};
       int [] c = {1, 3, 5, 6, 7};
       
       String [] strs1 = new String [3];
       strs1[0] = "Hllo";
       strs1[1] = "Java";
       strs1[2] = "World";
       
       String [] strs2 = {"Hello", "Java", "World"};
       ```
     
     - 생성과 따로 - 초기화 주의
       
       ```java
       int [] points;
       points = {1, 3, 4, 5, 6};   // 컴파일 오류
       
       in [] points;
       points = new int [] {1, 3, 4, 5, 6}; //선언할 때는 배열의 크기를 알 수 없을 때
       ```
   
   - 다차원 배열
     
     - `int[][] intArray;`, `int intArray[][];`, `int [] intArray [];`
