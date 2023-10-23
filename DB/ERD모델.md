## 데이터 모델링
비즈니스에서 수집하고 생성하는 각기 다른 모든 **데이터를 분석하고 정의**하는 것은 물론 **데이터 간의 관계도 분석**하고 정의하는 프로세스   
→ 이렇게 분석된 모델을 가지고 실제 데이터베이스를 생성하여 개발 및 데이터 관리에 사용함 
![erd_1.png](./image/erd_1.jpg)

<br></br>
### 1. 요구사항 수집 및 분석
어떤 데이터를 모델링할 것인지에 대한 요구사항 수집 및 분석 
<br></br>
### 2. 개념적 모델링
전체 모델에서 중요한 골격이 되는 엔티티와 관계 위주로 모델링     
→ 각 개체들과 개체 간의 관계를 표현하기 위해 ERD 다이어그램을 생성함

![erd_2.png](./image/erd_2.png)
<br></br>
### 3. 논리적 모델링
개념적 모델링에서 추출된 개체와 개체들의 관계를 구조적으로 설계하는 단계 
- 데이터에 대한 Key, 속성, 관계 등을 표시하며, 정규화 활동을 수행함
- 정규화를 통해 데이터모델의 일관성을 확보하고 중복을 제거하여 신뢰성 있는 데이터 구조를 얻을 수 있음

![erd_3.png](./image/erd_3.png)

### 4. 물리적 모델링
최종적으로 데이터를 관리할 데이터베이스를 선택하고, 논리적 모델을 특정 데이터베이스로 설계함 (실제 저장 공간, 분산, 저장 방법 등까지도 고려)     
-> 논리 모델을 각 DBMS의 특성에 맞게 데이터 베이스 저장 구조(물리 데이터 모델)로 변환
<br></br>
### 5. 코딩을 통해 실제 DB로 구현 

<br></br>

## ERD (Entity Relationship Diagram)
Entity(개체)와 Relationship(관계)를 중점적으로 표시함으로써 데이터베이스 구조를 한 눈에 알아볼 수 있게 하는 다이어그램     

ERD 표기법은 크게 Peter-Chen 표기법과 까마귀 발 표기법으로 나눌 수 있음     
- **Peter Chen 표기법** - 흔히 대학교에서 배우는 ERD 표기법으로 실무에서는 보통 사용되지 않음
  
  ![erd_18.png](./image/erd_18.png)
- **까마귀 발 표기법** - 표기 방법이 까마귀의 발을 닮았다고 하여 붙여진 이름! 실무에서 가장 많이 사용되는 방식으로 IE 표기법, Barker 표기법 등이 있음
  
  ![erd_20.jpeg](./image/erd_20.jpeg)      
  **IE 표기법**
  <br></br>
  ![erd_19.png](./image/erd_19.png)      
  **Barker 표기법**
<br></br>
### 엔티티
정의 가능한 사물 또는 개념(사람, 도서 정보 등) → 데이터베이스의 테이블이 엔티티로 표현됨
- IE 표기법 - 직사각형에 Entity 의 이름을 상단에 표기함
- Barker 표기법 - 꼭지점이 둥근 형태인 Soft Box로 표기함, 엔티티 이름은 박스 상단에 표기 

#### 속성(Attribute)
- IE 표기법 - 엔티티 이름 하단 좌측에/속성 옆에 괄호로 PK, FK 등의 정보를 표기하고, 우측에는 어트리뷰트의 이름을 표기
- Barker 표기법 - 식별자&필수 입력='#'로 표기, 일반 속성&필수 입력='*'로 표기, 일반 속성&선택적 입력='o'로 표기

#### 도메인(Domain)    
개념적 모델링 단계이기 때문에 도메인 작성은 선택적임!
    
<br></br>

### 릴레이션
#### 💡 식별자 vs 키
식별자는 논리적 모델링 관점으로 여러 개의 집합체를 담고 있는 하나의 통(엔티티)에서 각각을 구분할 수 있는 논리적인 이름   
키는 물리적 모델링 관점으로 데이터베이스 테이블에 접근하는 물리적 매개체 
<br></br> 

#### 식별자 관계와 비식별자 관계
- **식별자 관계 (실선)**      
  부모 엔티티의 기본키/유니크키를 자식 엔티티의 기본키로 사용하는 관계 -> 자식 엔티티는 부모 엔티티가 존재해야 존재할 수 있음      
  ex) 학생과 성적 간 관계 (성적 데이터는 학생이 존재해야 존재할 수 있음) 
<br></br>
- **비식별자 관계 (점선)**        
  부모 엔티티의 기본키/유니크키를 자식 엔티티의 외래키로 사용하는 관계 -> 자식 엔티티는 부모 엔티티의 존재유무와 관계 없이 독립적으로 존재할 수 있음      
  ex) 학생과 과목 간 관계 (수강하는 학생이 없어도 과목은 독립적으로 존재할 수 있음)

<br></br>
#### 카디널리티
관계가 존재하는 두 엔티티 사이 관계 → 한 엔티티에서 다른 엔티티의 몇 개의 개체와 대응되는지 제약조건을 선을 그어 표기함
- **One to One (1대 1)**
- **One to Many (1대 N)**
- **Many to Many (M대 N)**

![erd_21.png](./image/erd_21.png)   
**IE 표기법** 

![erd_22.png](./image/erd_22.png)  
**Barker 표기법** 
<br></br>
참고로 두 엔티티가 다대다 관계일 경우, 두 개의 엔티티만으로는 서로를 표현하는데 부족함 
데이터모델링에서는 M대N 관계를 완성되지 않은 모델로 간주하여, 두 엔티티의 관계를 1:N, N:1로 조정하는 작업이 필요함
즉 두 엔티티의 관련성을 표현하기 위해서는 중간에 또 다른 엔티티를 필요로 하며, 이 중간 엔티티가 두 엔티티의 공유 속성 역할을 하게 됨

![erd_15.png](./image/erd_15.png)
        
<br></br>

### 참여도
- IE 표기법 - ‘|’ 표시가 있는 곳은 반드시 있어야 하는 필수 엔티티, ‘O’ 표시가 있다면 없어도 되는 선택 엔티티
- Barker 표기법 - 필수 엔티티는 실선으로, 선택 엔티티는 점선으로 표기
  
![erd_23.png](./image/erd_23.png)

<br></br>
<br></br>
### 면접 질문 
1. 상황을 이해해서 ERD 설계 및 그리기
2. 그린 ERD에 따라 처리 프로세스 설명
<br></br>
### 출처
https://rutgo-letsgo.tistory.com/138     
[https://powerbi.microsoft.com/ko-kr/what-is-data-modeling/](https://powerbi.microsoft.com/ko-kr/what-is-data-modeling/)     
[https://inpa.tistory.com/entry/DB-📚-데이터-모델링-1N-관계-📈-ERD-다이어그램](https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%AA%A8%EB%8D%B8%EB%A7%81-1N-%EA%B4%80%EA%B3%84-%F0%9F%93%88-ERD-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8)     
https://dataonair.or.kr/db-tech-reference/d-guide/sql/?mod=document&uid=329      
[https://codedragon.tistory.com/8541](https://codedragon.tistory.com/8541)      
[https://snnchallenge.tistory.com/190](https://snnchallenge.tistory.com/190)    
https://hudi.blog/entity-relation-diagram/      
https://zaop.tistory.com/entry/데이터모델링-Barker바커-IE-표기법
