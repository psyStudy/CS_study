# 뷰(VIEW)

## 개념

사용자에게 접근이 허용된 데이터만을 제한적으로 보여주기 위해 하나 이상의 기본 테이블로부터 유도된 가상 테이블 (기본 테이블 = 뷰를 만드는 데 기반이 되는 물리적 테이블)

뷰는 데이터가 없고 SQL을 저장하고 있음

## 특징

- 뷰는 기본테이블로부터 유도된 테이블이기 때문에, 기본 테이블과 구조가 같음
- 가상테이블이기 때문에 저장장치 내에 **물리적으로 존재하진 않음**
- 필요한 데이터만 뷰로 정의해서 처리할 수 있기 때문에 관리가 용이하고 명령문이 간단해짐
- 기본 테이블의 PK를 포함한 속성 집합으로 뷰를 구성해야지만 삽입, 삭제, 갱신 연산이 가능함

ex) 아래 뷰는 insert 작업이 불가능함 (삽입 연산 시 기본 키에 null 값이 들어가기 때문에)

```sql
// 기본테이블은 student { sno(pk), sname, dept }
CREATE VIEW student_view
		AS sno, dept
		FROM student;

CREATE VIEW student_view
		AS sname, dept
		FROM student;
```

- 정의된 뷰는 다른 뷰의 정의의 기초가 될 수 있음
- 뷰를 삭제하면 뷰를 기초로 정의된 다른 뷰도 삭제되며, 기본테이블을 삭제하면 뷰와 뷰를 기초로 정의된 다른 뷰도 자동으로 삭제됨

## 장단점

### 장점

- 논리적 데이터 독립성을 제공함

```sql
**논리적 데이터 독립성이란?**
DBMS가 데이터베이스의 논리적 구조를 변경시키더라도 기존 응용 프로그램에 영향을 주지 않는 것

- 뷰에 새로운 Attribute가 추가 되어도 기본 테이블에는 영향을 주지 않음
- 반대로 기본테이블에 새로운 Attribute가 추가되어도 뷰에는 영향을 주지 않음
```

- 사용자의 데이터 관리를 간단하게 해줌
- 하나의 테이블로 여러개의 상이한 뷰를 정의할 수 있음
- 접근 제어를 통한 자동 보안이 제공됨

### 단점

- 독립적인 인덱스를 가질 수 없음
- 한번 정의된 뷰는 변경할 수 없으며, 삭제한 후에 다시 생성해야 함 (Alter 연산 불가능)
- 뷰로 구성된 내용에 대한 삽입, 삭제, 갱신 연산에 제약이 따름     
단순 뷰인 경우 INSERT, UPDATE, DELETE가 자유로우며 (NOT NULL 컬럼 주의)
함수, UNION, GROUP BY 등을 사용한 복합 뷰인 경우 INSERT, UPDATE, DELETE가 불가능함(조인만 사용한 복합 뷰인 경우 제한적으로 가능)

```sql
-- OR REPLACE : 해당 구문을 사용하면 뷰를 수정할 때 DROP 없이 수정이 가능함 
CREATE OR REPLACE VIEW v_emp 
AS
    SELECT a.empno, a.ename, a.job
				 , TO_CHAR(a.hiredate, 'YYYY-MM-DD') AS hiredate
         , b.deptno
      FROM emp a, dept b
      WHERE a.deptno = b.deptno
;

1.
INSERT INTO v_emp(empno, ename, deptno) VALUES(9999, 'TEST', 20)
ORA-01776: 조인 뷰에 의하여 하나 이상의 기본 테이블을 수정할 수 없습니다

INSERT INTO v_emp(empno, ename) VALUES(9999, 'TEST')
deptno 컬럼을 제외하면 정상적으로 입력됨
```

## SQL문법

### 뷰 정의문

```sql
-- MySQL, Oracle
CREATE VIEW 뷰이름[(속성이름)] AS SELECT문;

--고객 테이블에서 주소가 서울시인 고객들의 성명과 전화번호를 서울고객이라는 뷰로 만들어라--
CREATE VIEW 서울고객(성명, 전화번호)
AS SELECT 성명 전화번호
FROM 고객
WHERE 주소 = '서울시';
```

### 뷰 삭제문

> RESTRICT - 뷰를 다른곳에서 참조하고 있으면 **삭제가 취소됨**
CASCADE - 뷰를 참조하는 다른 뷰나 제약 조건까지 모두 삭제됨
> 

```sql
-- MySQL, Oracle
DROP VIEW 뷰이름 RESTRICT or CASCADE

--서울고객이라는 뷰를 삭제해라--
DROP VIEW 서울고객 RESTRICT;
```

### 면접질문

1. 뷰란 무엇입니까?
2. 뷰의 장점과 단점은 무엇입니까?

### 출처

[https://coding-factory.tistory.com/224](https://coding-factory.tistory.com/224)

[https://m.blog.naver.com/k97b1114/140153299419](https://m.blog.naver.com/k97b1114/140153299419)

[https://simuing.tistory.com/entry/정보처리기사-필기대비-공부노트-1데이터베이스](https://simuing.tistory.com/entry/%EC%A0%95%EB%B3%B4%EC%B2%98%EB%A6%AC%EA%B8%B0%EC%82%AC-%ED%95%84%EA%B8%B0%EB%8C%80%EB%B9%84-%EA%B3%B5%EB%B6%80%EB%85%B8%ED%8A%B8-1%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)

[http://jidum.com/jidums/view.do?jidumId=84](http://jidum.com/jidums/view.do?jidumId=84)

[https://victorydntmd.tistory.com/131](https://victorydntmd.tistory.com/131)

[https://blog.naver.com/zboizz/221698413228](https://blog.naver.com/zboizz/221698413228)

[https://thalals.tistory.com/346](https://thalals.tistory.com/346)

[https://gent.tistory.com/361](https://gent.tistory.com/361)
