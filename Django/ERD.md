# ERD Entity-Relationship Diagram

: 데이터베이스의 구조를 시각적으로 표현하는 구조

- Entity(개체), 속성, 엔티티간의 관계를 그래픽 형태로 나타내어 시스템의 논리적 구조를 모델링하는 다이어그램

- 구성요소
  
  - 엔티티(Entity)
    
    : 데이터베이스에 저장되는 객체나 개념
    
    - ex) 고객, 주문, 제품
  
  - 속성(Attribute)
    
    : 엔티티의 특성이나 성질
    
    - ex) 고객(이름, 주소, 전화번호)
  
  - 관계(Relationship)
    
    : 엔티티 간의 연관성
    
    - ex) 고객이 '주문'한 제품

- 개체와 속성
  
  - 개체 : 회원(User)
  
  - 속성 : 회원번호(id), 이름, 주소 등 -> 개체가 지닌 속성 및 속성의 데이터 타입

- Cardinality
  
  : 한 엔티티와 다른 엔티티 간의 수직 관계를 나타내는 표현
  
  - 주요 유형
    
    - 일대일 (one-to-one, 1:1)
    
    - 다대일 (many-to-one, N:1)
    
    - 다대다 (many-to-many, M:N)
  
  - 선의 끝부분에 표시되며, 일반적으로 숫자나 기호(까마귀 발)로 표현됨