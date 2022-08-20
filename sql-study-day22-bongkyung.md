## 16 - 1 PL/SQL 구조
<br/>

> 블록이란?

- PL/SQL은 데이터베이스 관련 특정 작업을 수행하는 명령어와 실행에 필요한 여러 요소를 정의하는 명령어 등으로 구성되며, 이러한 명령어를 모아 둔 PL/SQL 프로그램의 기본 단위를 블록(block)이라고 한다.

| 구성 키워드 | 필수/선택 | 설명 |
|:--------:|:--------:|:----:|
| DECLARE(선언부) | 선택 | 실행에 사용될 변수 ・ 상수 ・ 커서 등을 선언 |
| BEGIN(실행부) | 필수 | 조건문 ・ 반복문 ・ SELECT ・| BEGIN(실행부) | 필수 | 조건문 ・ 반복문 ・ SELECT ・ DML  함수 등을 정의 |
| EXCEPTION(예외 처리부) | 선택 | PL/SQL 실행 도중 발생하는 오류(예외 상황)를 해결하는 문장 기술 |

```sql
// 기본 형식
DECLARE
  [실행에 필요한 여러 요소 선언];
BEGIN
  [작업을 위해 실제 실행하는 명령어];
EXCEPTION
  [PL/SQL수행 도중 발생하는 오류 처리];
END;
```

- 선언부와 예외 처러부는 생략 가능하지만 실행부는 반드시 존재해야 한다.
- 필요에 따라 PL/SQL 블록 안에 다른 블록을 포함할 수도 있다. 이를 중첩 블록(nested block)이라고 한다.

<br/>

> Hello, PL/SQL 출력하기

- PL/SQL 실행 결과를 화면에 출력하기 위해서는 SERVEROUTPUT 환경 변수 값을 ON으로 변경해야 한다.
- PUT_LINE은 화면 출력을 위해 오라클에서 기본으로 제공하며 DBMS_OUTPUT 패키지에 속해 있다.

```sql
SET SERVEROUTPUT ON; -- 실행 결과를 화면에 출력

BEGIN
   DBMS_OUTPUT.PUT_LINE('Hello, PL/SQL!');
END;
/
```

- PL/SQL 문을 작성하고 실행하기 위해 다음 사항을 기억하자

1. PL/SQL 블록을 구성하는 DECLARE, BEGIN, EXCEPTION 키워드에는 세미콜론(;)을 사용하지 않습니다.
2. PL/SQL 블록의 각 부분에서 실행해야 하는 문장 끝에는 세미콜론(;)을 사용합니다.
ex) DBMS_OUTPUT.PUT_LINE('Hello, PL/SQL!');
3. PL/SQL문 내부에서 한 줄 주석(--)과 여러 줄 주석(/\* ~ \*/)을 사용할 수 있습니다. 그리고 이들 주석은 SQL문에서도 사용할 수 있습니다.
4. PL/SQL문 작성을 마치고 실행하기 위해 마지막에 슬래시(/)를 사용합니다.

▶︎ PL/SQL문을 작성하고 실행하기 전에 SET SERVEROUTPUT ON 명령어를 실행하여 PL/SQL문 결과를 화면에 출력하는 것을 이자 말자.

<br/>

> PL/SQL 주석

- PL/SQL 주석은 PL/SQL 코드에 포함되어 있지만 실행되지 않는 문장을 뜻한다.

| 종류 | 사용 기호 | 설명 |
|:----:|:-------:|:-----:|
| 한 줄 주석 | -- [주석 처리 내용] | 현재 줄만 주석 처리됩니다. |
| 여러 줄 주석 | /\* <br/> [주석 처리 내용] <br/> \*/ | /\*에서 \*/까지 여러 줄에 걸쳐 주석 처리됩니다. |

<br/>

## 16 - 2 변수와 상수
<br/>

> 변수 선언과 값 대입하기

- 변수(variable)는 데이터를 일시적으로 저장하는 요소로 이름과 저장할 자료형을 지정하여 선언부(DECLARE)에서 작성한다.
- 선언부에서 작성한 변수는 실행부(BEGIN)에서 활용한다.

#### 기본 변수 선언과 사용

```sql
// 기본 형식
변수 이름 자료형 := 값 또는 값이 도출되는 여러 표현식;
// 1. 변수이름
// 2. 자료형
// 3. :=
// 4. 값 또는 값이 도출되는 여러 표현식
```

| 번호 | 설명 |
|:----:|:-------------:|
| 1. | 데이터를 저장할 변수 이름을 지정합니다. 이 변수 이름을 통해 저장한 데이터를 사용하게 됩니다. |
| 2. | 선언한 변수에 저장할 데이터의 자료형을 지정합니다. |
| 3. | 선언한 변수에 값을 할당하기 위해 :=를 사용합니다. 이 기호는 오른쪽 값을 왼쪽 변수에 대입하겠다는 뜻입니다. 값을 할당하지 않고 변수 선언만 한다면 3, 4는 생략할 수 있습니다. |
| 4. | 변수에 저장할 첫 데이터 값이나 저장할 수 있는 값이 결과로 반환되는 표현식을 지정합니다. 이 값은 변수에 지정한 자료형과 맞아야 합니다. |

#### 상수 정의하기

- 저장한 값이 필요에 따라 변하는 변수와 달리 상수(constant)는 한번 저장한 값이 프로그램이 종료될 때까지 유지되는 저장 요소이다.
- 상수를 선언할 때 다음과 같이 기존 변수 선언에 CONSTANT 키워드를 지정한다.

```sql
// 기본 형식
변수 이름 CONSTANT 자료형 := 값 또는 값을 도출하는 여러 표현식;

// 1. 변수이름
// 2. CONSTANT
// 3. 자료형
// 4. :=
// 5. 값 또는 값을 도출하는 여러 표현식
```

| 번호 | 설명 |
|:---:|:-------------:|
| 1. | 데이터를 저장할 변수 이름을 지정합니다. 이 변수 이름을 통해 저장한 데이터를 사용하게 됩니다. |
| 2. | 선언한 변수를 상수로 정의합니다. 즉 한번 저장한 값은 변하지 않습니다. |
| 3. | 선언한 변수에 저장할 데이터의 자료형을 지정합니다. |
| 4. | 선언한 변수에 값을 할당하기 위해 :=를 사용합니다. 이 기호는 오른쪽 값을 왼쪽 변수에 대입하겠다는 뜻입니다. 값을 할당하지 않고 변수를 선언만 한다면 3, 4는 생략할 수 있습니다. |
| 5. | 변수에 저장할 첫 데이터 값이나 저장할 수 있는 값을 결과로 반환하는 표현식을 지정합니다. 이 값은 변수에 지정한 자료형과 맞아야 합니다. |

#### 변수의 기본값 지정하기

```sql
// 기본 형식
변수 이름 자료형 DEFAULT 값 또는 값 도출되는 여러 표현식;

// 1. 변수이름
// 2. 자료형
// 3. DEFAULT
// 4. 값 또는 값 도출되는 여러 표현식
```

| 번호 | 설명 |
|:----:|:--------------:|
| 1. | 데이터를 저장할 변수 이름을 지정합니다. 이 변수 이름을 통해 저장한 데이터를 사용하게 됩니다. |
| 2. | 선언한 변수에 저장할 데이터의 자료형을 지정합니다. |
| 3. | DEFAULT 키워드를 작성하여 변수의 기본값을 명시합니다. |
| 4. | 변수에 저장할 첫 데이터 값이나 저장할 수 있는 값을 결과로 반환하는 표현식을 지정합니다. 이 값은 변수에 지정한 자료형과 맞아야 합니다. |

#### 변수에 NULL 값 저장 막기

- 특정 변수에 NULL이 저장되지 않게 하려면 NOT NULL 키워드를 사용한다.
- PL/SQL에서 선언한 변수는 특정 값을 할당하지 않으면 NULL값이 기본으로 할당된다.
- NOT NULL 키워드를 사용한 변수는 반드시 선언과 동시에 특정 값을 지정해 주어야 한다.

```sql
// 기본 형식
변수 이름 자료형 NOT NULL := 또는 DEFAULT 값 또는 값이 도출되는 여러 표현식;

// 1. 변수 이름
// 2. 자료형
// 3. NOT NULL
// 4. := 또는 DEFAULT
// 5. 값 또는 값이 도출되는 여러 표현식
```

| 번호 | 설명 |
|:----:|:-------------:|
| 1. | 데이터를 저장할 변수 이름을 지정합니다. 이 변수 이름을 통해 저장한 데이터를 사용하게 됩니다. |
| 2. | 선언한 변수에 저장할 데이터의 자료형을 지정합니다. |
| 3. | NOT NULL 키워드를 작성하여 변수에 NULL이 저장되지 못하도록 막습니다. |
| 4, 5. | 변수에 저장할 첫 데이터 값이나 저장할 수 있는 값을 결과로 반환하는 표현식을 지정합니다.<br/> 이 값은 변수에 지정한 자료형과 맞아야 합니다. 그리고 NULL이 아닌 값을 할당해야 합니다. DEFAULT 키워드를 함께 사용하여 변수의 기본값으로 설정할 수도 있습니다. |

#### 변수 이름 정하기

- 변수를 포함한 PL/SQL문에서 지정하는 객체 이름을 식별자(identifier)라고 한다.
- 식별자에 이름을 붙이는 규칙은 다음과 같다.
  
  1. 같은 블록 안에서 식별자는 고유해야 하며 중복될 수 없습니다.
  2. 대 ・ 소문자를 구별하지 않습니다.
  3. 테이블 이름 붙이는 규칙과 같은 규칙을 따릅니다.
     1. 이름은 문자로 시작해야 합니다(한글도 가능하며 숫자로 시작할 수 없음).
     2. 이름은 30byte 이하여야 합니다(영어는 30자, 한글은 15자까지 사용 가능).
     3. 이름은 영문자(한글 가능), 숫자(0-9), 특수문자($,#,\_)를 사용할 수 있습니다.
     4. SQL 키워드는 테이블 이름으로 사용할 수 없습니다(SELECT, FROM 등은 테이블 이름으로 사용 불가).
     
<br/>

> 변수의 자료형

- 변수에 저장할 데이터가 어떤 종류인지를 특정 짓기 위해 사용하는 자료형은 크게 스칼라(scalar), 복합(composite), 참조(reference), LOB(Large OBject)로 구분된다.

#### 스칼라형

- 스칼라형(scalar type)은 숫자, 문자열, 날짜 등과 같이 오라클에서 기본으로 정의해 놓은 자료형으로 내부 구성 요소가 없는 단일 값을 의미한다.

- 스칼라형은 숫자 ・ 문자열 ・ 날짜 ・ 논리 데이터로 나뉘며 대표적인 스칼라형은 다음과 같다.

| 분류 | 자료형 | 설명 |
|:---:|:-----:|:--------:|
| 숫자 | NUMBER | 소수점을 포함할 수 있는 최대 38자리 숫자 데이터 |
| 문자열 | CHAR | 최대 32,767바이트 고정 길이 문자열 데이터 |
| | VARCHAR2 | 최대 32,767바이트 가변 길이 문자열 데이터 |
| 날짜 | DATE | 기원전 4712년 1월 1일부터 서기 9999년 12월 31일까지 날짜 데이터 |
| 논리 데이터 | BOOLEAN | PL/SQL에서만 사용할 수 있는 논리 자료형으로 true, false, NULL을 포함 |

#### 참조형

- 참조형(reference type)은 오라클 데이터베이스에 존재하는 특정 테이블 열의 자료형이나 하나의 행 구조를 참조하는 자료형이다.
- 열을 참조할 때 %TYPE, 행을 참조할 때 %ROWTYPE을 사용한다.

- %TYPE으로 선언한 변수는 지정한 테이블 열과 완전히 같은 자료형이 된다.
```sql
// 기본 형식
변수 이름 테이블이름.열이름%TYPE;

// 1. 변수 이름
// 2. 테이블이름.열이름
// 3. %TYPE
```

| 번호 | 설명 |
|:---:|:--------:|
| 1. | 데이터가 저장될 변수의 이름을 지정합니다. 이 변수 이름을 통해 저장된 데이터를 사용하게 됩니다. |
| 2. | 특정 테이블에 속한 열의 이름을 명시합니다. 번호 1. 변수는 명시된 테이블의 열과 같은 크기의 자료형이 지정됩니다. |
| 3. | 앞에서 지정한 테이블의 열과 같은 자료형 및 크기임을 명시합니다. 이후 := 또는 DEFAULT 키워드를 사용하여 값을 먼저 지정해 줄 수도 있습니다. |

- 특정 테이블에서 하나의 열이 아닌 행 구조 전체를 참조할 때 %ROWTYPE을 사용한다.

```sql
// 기본 형식
변수 이름 테이블 이름%ROWTYPE;

// 1. 변수 이름
// 2. 테이블 이름
// 3. %ROWTYPE
```

| 번호 | 설명 |
|:---:|:------------:|
| 1. | 데이터를 저장할 변수 이름을 지정합니다. 이 변수 이름을 통해 저장한 데이터를 사용하게 됩니다. |
| 2. | 특정 테이블을 지정합니다. 번호 1. 변수는 명시된 테이블 열과 같은 종류의 데이터를 가지게 됩니다. |
| 3. | %TYPE과는 달리 저장할 값을 직접 지정할 수 없습니다. |

#### 복합형, LOB형

- 스칼라형과 참조형 외에도 PL/SQL에서는 복합형(composite type)과 LOB형을 사용할 수 있다.

| 분류 | 자료형 | 설명 |
|:----:|:-----:|:-----------:|
| 컬렉션 | TABLE | 한 가지 자료형의 데이터를 여러 개 저장(테이블의 열과 유사) |
| 레코드 | RECORD | 여러 종류 자료형의 데이터를 저장(테이블의 행과 유사) |

- Large Object를 의미하는 LOB형은 대용량의 텍스트 ・ 이미지 ・ 동영상 ・ 사운드 데이터 등 대용량 데이터를 저장하기 위한 자료형으로 대표적으로 BLOB, CLOB 등이 있다.

---


    