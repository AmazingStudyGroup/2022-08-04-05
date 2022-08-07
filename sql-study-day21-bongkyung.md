## 15 - 1 사용자 관리

<br/>

> 사용자란?

- 오라클 데이터베이스에서는 데이터베이스에 접속하여 데이터를 관리하는 계정을 사용자(USER)로 표현한다.

<br/>

> 데이터베이스 스키마란?

- 데이터베이스에서 데이터 간 관계, 데이터 구조, 제약 조건 등 데이터를 저장 및 관리하기 위해 정의한 데이터베이스 구조의 범위를 스키마(schema)를 통해 그룹 단위로 분류한다.

<br/>

> 사용자 생성

- 오라클 사용자를 생성할 때는 CREATE TABLE문을 사용한다. 다음과 같이 명령어에는 사용할 수 있는 옵션이 여러 가지 있다.
- 기본적으로 사용자 이름과 패스워드만 지정해 주면 사용자를 생성할 수 있다.

```sql
CREATE USER 사용자 이름(필수)
IDENTIFIED BY 패스워드(필수)
DEFAULT TABLESPACE 테이블 스페이스 이름(선택)
TEMPORARY TABLESPACE 테이블 스페이스(그룹) 이름(선택)
QUOTA 테이블 스페이스크기 ON 테이블 스페이스 이름(선택)
PROFILE 프로파일 이름(선택)
PASSWORD EXPIRE(선택)
ACCOUNT [LOCK/UNLOCK](선택);
```

- 사용자 생성은 일반적으로 데이터베이스 관리 권한을 가진 사용자가 권한을 가지고 있다. 오라클 데이터베이스를 설치할 때 자동으로 생성된 SYS, SYSTEM이 데이터베이스 관리 권한을 가진 사용자이다.

<br/>

> 사용자 정보 조회

```sql
// 1.
SELECT * FROM ALL_USERS
WHERE USERNAME = 'ORCLSTUDY';

// 2.
SELECT * FROM DBA_USERS
WHERE USERNAME = 'ORCLSTUDY';

// 3.
SELECT * FROM DBA_OBJECTS
WHERE OWNER = 'ORCLSTUDY';
```

<br/>

> 오라클 사용자의 변경과 삭제

```sql
// 사용자 정보(패스워드) 변경하기
ALTER USER ORCLSTUDY
IDENTIFIED BY ORCL;
```

```sql
// 사용자 삭제하기
DROP USER ORCLSTUDY;
```

```sql
// 사용자 스키마에 객체가 있을 경우에 CASCADE 옵션을 사용하여 사용자와 객체를 모두 삭제할 수 있다.
// 사용자와 객체 모두 삭제하기
DROP USER ORCLSTUDY CASCADE;
```

<br/>

## 15 - 2 권한 관리
<br/>

- 오라클에서는 권한을 시스템 권한(system privilege)과 객체 권한(object privilege)으로 분류하고 있다.

<br/>

> 시스템 권한이란?

- 오라클 데이터베이스의 시스템 권한은 사용자 생성과 정보 수정 및 삭제, 데이터베이스 접근, 오라클 데이터베이스의 여러 자원과 객체 생성 및 관리 등의 권한을 포함한다.
- ANY 키워드가 들어 있는 권한은 소유자에 상관없이 사용 가능한 권한을 의미한다.

| 시스템 권한 분류 | 시스템 권한 | 설명 |
|:-----:|:------:|:------:|
| USER(사용자) | CREATE USER | 사용자 생성 권한 |
| | ALTER USER | 생성된 사용자의 정보 수정 권한 |
| | DROP USER | 생성된 사용자의 삭제 권한 |
| SESSION(접속) | CREATE SESSION | 데이터베이스 접속 권한 |
| | ALTER SESSION | 데이터베이스 접속 상태에서 환경 값 변경 권한 |
| TABLE(테이블) | CREATE TABLE | 자신의 테이블 생성 권한 |
| | CREATE ANY TABLE | 임의의 스키마 소유 테이블 생성 권한 |
| | ALTER ANY TABLE | 임의의 스키마 소유 테이블 수정 권한 |
| | DROP ANT TABLE | 임의의 스키마 소유 테이블 삭제 권한 |
| | INERT ANY TABLE | 임의의 스키마 소유 테이블 데이터 삽입 권한 |
| | UPDATE ANY TABLE | 임의의 스키마 소유 테이블 데이터 수정 권한 |
| | DELETE ANY TABLE | 임의의 스키마 소유 테이블 데이터 삭제 권한 |
| | SELECT ANY TABLE | 임의의 스키마 소유 테이블 데이터 조회 권한 |
| INDEX(인덱스) | CREATE ANY INDEX | 임의의 스키마 소유 테이블의 인덱스 생성 권한 |
| | ALTER ANY INDEX | 임의의 스키마 소유 테이블의 인덱스 수정 권한 |
| | DROP ANY INDEX | 임의의 스키마 소유 테이블의 인덱스 삭제 권한 |
| VIEW(뷰) | (생략) | 뷰와 관련된 여러 권한 |
| SEQUENCE(시퀀스) | (생략) | 시퀀스와 관련된 여러 권한 |
| SYNONYM(동의어) | (생략) | 동의어와 관련된 여러 권한 |
| RPOFILE(프로파일) | (생략) | 사용자 접속 조건 지정과 관련된 여러 권한 |
| ROLE(롤) | (생략) | 권한을 묶은 그룹과 관련된 여러 권한 |

<br/>

> 시스템 권한 부여

```sql
// 기본 형식
GRANT 1.[시스템 권한] TO 2.[사용자 이름/롤(Role)이름/PUBLIC]
3.[WITH ADMIN OPTION];
```

| 번호 | 설명 |
|:-----:|:---------------:|
| 1. | 오라클 데이터베이스에서 제공하는 시스템 권한을 지정합니다. 한 번에 여러 종류의 권한을 부여하려면 쉼표(,)로 구분하여 권한 이름을 여러 개 명시해 주면 됩니다(필수). |
| 2. | 권한을 부여하려는 대상을 지정합니다. 사용자 이름을 지정해 줄 수도 있고, 이후 소개할 롤(role)을 지정할 수도 있습니다. 여러 사용자 또는 롤에 적용할 경우 쉼표(,)로 구분합니다. PUBLIC은 현재 오라클 데이터베이스의 모든 사용자에게 권한을 부여하겠다는 의미입니다(필수). |
| 3. | WITH ADMIN OPTION은 현재 GRANT문을 통해 부여받은 권한을 다른 사용자에게 부여할 수 있는 권한도 함께 부여받습니다. 현재 사용자가 권한이 사라져도, 권한을 재부여한 다른 사용자의 권한은 유지됩니다(선택).

<br/>

> 시스템 권한 취소

- GRANT 명령어로 부여한 권한의 취소는 REVOKE 명령어를 사용한다.

```sql
// 기본 형식
REVOKE [시스템 권한] FROM [사용자 이름/롤(role)이름/PUBLIC];
```

<br/>

> 객체 권한이란?

- 객체 권한은 특정 사용자가 생성한 테이블∙인덱스∙뷰∙시퀀스 등과 관련된 권한이다.

| 객체 권한 분류 | 객체 권한 | 설명 |
|:-----:|:-----:|:-----:|
| TABLE(테이블) | ALTER | 테이블 변경 권한 |
| | DELETE | 테이블 데이터 삭제 권한 |
| | INDEX | 테이블 인덱스 생성 권한 |
| | INSERT | 테이블 데이터 삽입 권한 |
| | REFERENCES | 참조 데이터 생성 권한 |
| | SELECT | 테이블 조회 권한 |
| | UPDATE | 테이블 데이터 수정 권한 |
| VIEW(뷰) | DELETE | 뷰 데이터 삭제 권한 |
| | INSERT | 뷰 데이터 삽입 권한 |
| | REFERENCES | 참조 데이터 생성 권한 |
| | SELECT | 뷰 조회 권한 |
| | UPDATE | 뷰 데이터 수정 권한 |
| SEQUENCE(시퀀스) | ALTER | 시퀀스 수정 권한 |
| | SELECT | 시퀀스의 CURRVAL과 NEXTVAL 사용 권한 |
| PROCEDURE(프로시저) | (생략) | 프로시저 관련 권한 |
| FUNCTION(함수) | (생략) | 함수 관련 권한 |
| PACKAGE(패키지) | (생략) | 패키지 관련 권한 |

<br/>

> 객체 권한 부여

```sql
// 기본 형식
GRANT [객체 권한/ALL PRIVILEGES] // 1.
   ON [스키마.객체 이름] // 2.
   TO [사용자 이름/롤(role)이름/PUBLIC] // 3.
   [WITH GRANT OPTION]; // 4.
```

| 번호 | 설명 |
|:----:|:--------------:|
| 1. | 오라클 데이터베이스에서 제공하는 객체 권한을 지정합니다. 한 번에 여러 종류의 권한을 부여하려면 쉼표(,)로 구분하여 권한을 여러 개 명시해 주면 됩니다. ALL PRIVILEGES는 객체의 모든 권한을 부여함을 의미합니다(필수). |
| 2. | 권한을 부여할 대상 객체를 명시합니다(필수). |
| 3. | 권한을 부여하려는 대상을 지정합니다. 사용자 이름을 지정해 줄 수도 있고 이후 소개할 롤(role)을 지정할 수도 있습니다. 여러 사용자 또는 롤에 적용할 경우 쉼표(,)로 구분합니다. PUBLIC은 현재 오라클 데이터베이스의 모든 사용자에게 권한을 부여하겠다는 의미입니다(필수). |
| 4. | WITH GRANT OPTION은 현재 GRANT문을 통해 부여받은 권한을 다른 사용자에게 부여할 수 있는 권한도 함께 부여받습니다. 현재 권한을 부여받은 사용자의 권한이 사라지면, 다른 사용자에게 재부여된 권한도 함께 사라집니다(선택). |

<br/>

> 객체 권한 취소

```sql
// 기본 형식
REVOKE [객체 권한/ALL PRIVILEGES](필수)
    ON [스키마.객체 이름](필수)
  FROM [사용자 이름/롤(role)이름/PUBLIC](필수)
[CASCADE CONSTRAINTS/FROCE](선택);
```

<br/>

## 15 - 3 롤 관리
<br/>

> 롤이란?

- 롤은 여러 종류의 권한을 묶어 놓은 그룹을 뜻한다.
- 롤을 사용하면 여러 권한을 한 번에 부여하고 해제할 수 있으므로 권한 관리 효율을 높일 수 있다.
- 롤은 오라클 데이터베이스를 설치할 때 기본으로 제공되는 사전 정의된 롤(predefined roles)과 사용자 정의 롤(user roles)로 나뉜다.

<br/>

> 사전 정의된 롤

- CONNECT 롤

사용자가 데이터베이스에 접속하는 데 필요한 CREATE SESSION 권한을 가지고 있다.

```sql
ALTER SESSION, CREATE CLUSTER, CREATE DATABASE LINK, CREATE SEQUENCE, CREATE SESSION, CREATE SYNONYM, CREATE TABLE, CREATE VIEW
```

- RESOURCE 롤

사용자가 테이블, 시퀀스를 비롯한 여러 객체를 생성할 수 있는 기본 시스템 권한을 묶어 놓은 룰이다.

```sql
CREATE TRIGGER, CREATE SEQUENCE, CREATE TYPE, CREATE PROCEDURE, CREATE CLUSTER, CREATE OPERATOR, CREATE INDEXTYPE, CREATE TABLE
```

- DBA 롤

데이터베이스를 관리하는 시스템 권한을 대부분 가지고 있다. 오라클 11g 버전 기준 202개 권한을 가진 매우 강력한 롤이다.

<br/>

> 사용자 정의 롤

- 사용자 정의 롤은 필요에 의해 직접 권한을 포함시킨 롤을 뜻한다.
  
  1. CREATE ROLE문으로 롤을 생성합니다.
  2. GRANT 명령어로 생성한 롤에 권한을 포함시킵니다.
  3. GRANT 명령어로 권한이 포함된 롤을 특정 사용자에게 부여합니다.
  4. REVOKE 명령어로 롤을 취소시킵니다.

<br/>

---

