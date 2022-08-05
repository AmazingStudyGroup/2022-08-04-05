## 16 PL/SQL 기초

### 16-1 PL/SQL 구조

#### 블록이란?
- PL/SQL은 데이터베이스 관련 특정 작업을 수행하는 명령어와 실행에 필요한 여러 요소를 정의하는 명령어 등으로 구성됨     
- 이러한 명령어를 모아 둔 PL/SQL 프로그램의 기본 단위를 블록(block)이라고 함     

|구성 키워드|필수/선택|설명|
|----|----|----|
|DECLARE(선언부)|선택|실행에 사용될 변수, 상수, 커서 등을 선언|
|BEGIN(실행부)|필수|조건문, 반복문, SELECT, DML, 함수 등을 정의|
|EXCEPTION(예외처리부)|선택|PL/SQL 실행 도중 발생하는 오류(예외상황)를 해결하는 문장 기술|

```sql
DECLARE -- 선언부
  [실행에 필요한 여러 요소 선언];
BEGIN -- 실행부 : 반드시 존재해야 함
  [작업을 위해 실제 실행하는 명령어];
EXCEPTION -- 예외처리부
  [PL/SQL수행 도중 발생하는 오류 처리];
END;
```

> PL/SQL 블록 안에 다른 블록을 포함할 수도 있는데, 이를 '중첩블록(nested block)'이라고 한다.     

#### Hello, PL/SQL 출력하기
- 실습에 앞서, SERVEROUTPUT 환경 변수 값을 ON 으로 변경해야 함    

```sql
SET SERVEROUTPUT ON; -- 실행 결과를 화면에 출력
BEGIN
 DBMS_OUTPUT.PUT_LINE('Hello, PL/SQL!');
END;
/ 
```

- PL/SQL 작성 및 실행을 위해 기억할 부분     
1. PL/SQL 블록을 구성하는 DECLARE, BEGIN, EXCEPTION 키워드에는 세미콜론(;)을 사용하지 않는다.    
2. PL/SQL 블록의 각 부분에서 실행해야 하는 문장 끝에는 세미콜론(;)을 사용한다.      
`DBMS_OUTPUT.PUT_LINE('Hello, PL/SQL!');`
3. PL/SQL문 내부에서 한 줄 주석과 여러 줄 주석을 사용할 수 있다. SQL문에서도 사용할 수 있다.     
4. PL/SQL문 작성을 마치고 실행하기 위해 마지막에 슬래시(/)를 사용한다.     

#### PL/SQL 주석
- PL/SQL 코드에 포함되어 있지만 실행되지 않는 문장    
```sql
-- 한줄 주석 사용하기

DECLARE
V_EMPNO NUMBER(4) := 7788;
V_ENAME VARCHAR2(10);
BEGIN
V_ENAME := 'SCOTT';
-- DBMS_OUTPUT.PUT_LINE('V_EMPNO : ' || V_EMPNO); 
DBMS_OUTPUT.PUT_LINE('V_ENAME : ' || V_ENAME);
END;
/
```
결과화면 `V_ENAME:SCOTT`

```sql
-- 여러 줄 주석 사용하기

DECLARE
V_EMPNO NUMBER(4) := 7788;
V_ENAME VARCHAR2(10);
BEGIN
V_ENAME := 'SCOTT';
/*
DBMS_OUTPUT.PUT_LINE('V_EMPNO : ' || V_EMPNO); 
DBMS_OUTPUT.PUT_LINE('V_ENAME : ' || V_ENAME);
*/
END;
/
```
> 아무것도 출력되지 않음


### 16-2 변수와 상수

### 변수 선언과 값 대입하기
- 변수는 데이터를 일시적으로 저장하는 요소로 이름과 저장할 자료형을 지정하여 선언부(DECLARE)에서 작성한다.     
- 선언부에 작성한 변수는 실행부(BEGIN)에서 활용한다.      

**기본 변수 선언과 사용**      
`변수 이름 자료형 := 값 또는 값이 도출되는 여러 표현식;`     

**상수 정의하기**     
`변수 이름 CONSTANT 자료형 := 값 또는 값을 도출하는 여러 표현식;`         
> 상수는 한 번 저장한 값이 프로그램이 종료될 때까지 유지되는 저장 요소이다.      

```sql
-- 상수에 값을 대입한 후 출력하기 
DECLARE
  V_TAX CONSTANT NUMBER(1) := 3;
BEGIN
  DBMS_OUTPUT.PUT_LINE('V_TEX : ' || V_TAX);
END;
/
```

**변수의 기본값 지정하기** 

```sql
DECLARE
  V_DEPTNO NUMBER(2) DEFAULT 10;
BEGIN
  DBMS_OUTPUT.PUT_LINE('V_DEPTNO : ' || V_DEPTNO);
END;
/
```

결과화면 `V_DEPTNO:10`

**변수에 NULL값 저장 막기**     
- PL/SQL에서 선언한 변수는 특정 값을 할당하지 않으면 NULL값이 기본으로 할당된다    
- 때문에 NOT NULL 키워드를 사용한 변수는 반드시 선언과 동시에 특정 값을 지정해줘야 한다      

```sql
-- 변수에 NOT NULL을 설정하고 값을 대입한 후 출력하기 
DECLARE
  V_DEPTNO NUMBER(2) NOT NULL := 10;
BEGIN
  DBMS_OUTPUT.PUT_LINE('V_DEPTNO : ' || V_DEPTNO);
END;
/

-- 변수에 NOT NULL 및 기본값을 설정한 후 출력하기    
DECLARE
  V_DEPTNO NUMBER(2) NOT NULL DEFAULT 10;
BEGIN
  DBMS_OUTPUT.PUT_LINE('V_DEPTNO : ' || V_DEPTNO);
END;
/
```

**변수 이름 정하기**          
변수를 포함한 PL/SQL문에서 지정하는 객체 이름을 식별자(identifier)라고 한다.     
```
// 식별자에 이름 붙이는 규칙
1. 같은 블록 안에서 식별자는 고유해야 하며 중복될 수 없다.    
2. 대소문자를 구별하지 않는다.    
3. 테이블 이름 붙이는 규칙과 같은 규칙을 따른다. 
  1) 이름은 문자로 시작해야 한다(한글도 가능하며 숫자로 시작할 수 없음)
  2) 이름은 30byte 이하 (영어는 30자, 한글은 15자 까지 사용가능)
  3) 이름은 영문자(한글 가능), 숫자(0-9), 특수문자($, #, _)를 사용할 수 있음
  4) SQL 키워드는 테이블 이름으로 사용할 수 없음(SELECT, FROM 등은 테이블 이름으로 사용 불가)
```

#### 변수의 자료형
스칼라, 복합, 참조, LOB    
  
- 스칼라형     
스칼라형은 숫자, 문자열, 날짜 등과 같이 오라클에서 기본으로 정의해 놓은 자료형으로 내부 구성 요소가 없는 단일 값을 의미한다.        
> 스칼라형은 C, Java 등의 프로그래밍 언어에서 primitive 자료형과 유사하다.      

- 참조형     
  - 오라클 데이터베이스에 존재하는 특정 테이블 열의 자료형이나 하나의 행 구조를 참조하는 자료형     
  - 열을 참조할 때 %TYPE, 행을 참조할 때 %ROWTYPE      

```sql
-- 참조형(열)의 변수에 값을 대입한 후 출력하기 

DECLARE
  V_DEPTNO DEPT.DEPTNO%TYPE := 50;
BEGIN
  DBMS_OUTPUT.PUT_LINE('V_DEPTNO : ' || V_DEPTNO);
END;  
/
```

> 특정 테이블에서 하나의 열이 아닌 행 구조 전체를 참조할 때 %ROWTYPE을 사용한다.      

```sql
-- 참조형(행)의 변수에 값을 대입한 후 출력하기

DECLARE
  V_DEPT_ROW DEPT%ROWTYPE;
BEGIN
  SELECT DEPTNO, DNAME, LOC INTO V_DEPT_ROW
    FROM DEPT
   WHERE DEPTNO = 40;
  DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || V_DEPT_ROW.DEPTNO);
  DBMS_OUTPUT.PUT_LINE('DNAME : ' || V_DEPT_ROW.DNAME;
  DBMS_OUTPUT.PUT_LINE('LOC : ' || V_DEPT_ROW.LOC);
END;
/
```

- 복합형, LOB형
   - 복합형 : 여러 종류 및 개수의 데이터를 저장하기 위해 사용자가 직접 정의하는 자료형(컬렉션, 레코드로 구분됨)      
   - LOB(Large Object) : 대용량의 텍스트, 이미지, 동영상, 사운드 데이터 등 대용량 데이터를 저장하기 위한 자료형(BLOB, CLOB 등)
