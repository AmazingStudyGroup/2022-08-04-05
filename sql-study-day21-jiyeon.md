## 15 사용자, 권한, 롤 관리
### 15-1 사용자 관리
#### 사용자란?
데이터베이스에 접속하여 데이터를 관리하는 계정을 사용자(USER)로 표현한다.    

#### 데이터베이스 스키마란?
데이터베이스에서 데이터 간 관계, 데이터 구조, 제약 조건 등 데이터를 저장 및 관리하기 위해 정의한 데이터베이스 구조의 범위를 스키마를 통해 그룹 단위로 분류한다.     

#### 사용자 생성
```sql
-- SCOTT 계정으로 사용자 생성하기(권한이 없어서 오류남)
-- 사용자 생성은 일반적으로 데이터베이스 관리 권한을 가진 사용자(SYS, SYSTEM)가 권한을 가진다. 

CREATE USER ORCLSTUDY
IDENTIFIED BY ORACLE;

-- SYSTEM 사용자로 접속 후 사용자 생성하기(SQL*PLUS)
CREATE USER ORCLSTUDY
IDENTIFIED BY ORACLE;

-- 새로 생성한 사용자에 접속을 시도하면 안되는 이유는
-- 사용자가 생성되긴 했지만 데이터베이스 연결을 위한 권한을 부여받지 못헀기 때문이다.

-- SYSTEM 사용자로 접속 후 ORCLSTUDY 사용자에게 권한 부여하기
GRANT CREATE SESSION TO ORCLSTUDY;
```
#### 사용자 정보 조회(데이터 사전을 활용)
```sql
SELECT * FROM ALL_USERS
 WHERE USERNAME = 'ORCLSTUDY';

SELECT * FROM DBA_USERS
 WHERE USERNAME = 'ORCLSTUDY';

SELECT * FROM DBA_OBJECTS
 WHERE OWNER = 'ORCLSTUDY';
```

#### 오라클 사용자의 변경과 삭제
```sql
-- 사용자 정보(패스워드) 변경하기 (물론 이것도 SYSTEM 사용자로 수행 가능)
ALTER USER ORCLSTUDY
IDENTIFIED BY ORCL;

-- 사용자 삭제하기 (만약 삭제하려는 사용자가 접속되어 있는 상태라면 삭제 불가)
DROP USER ORCLSTUDY;

-- 오라클 사용자와 객체 모두 삭제
DROP USER ORCLSTUDY CASCADE;
```

### 15-2 권한 관리
#### 시스템 권한이란?
- 사용자 생성과 정보 수정 및 삭제, 데이터베이스 접근, 오라클 데이터베이스의 여러 자원과 객체 생성 및 관리 등의 권한을 포함한다.     
- 데이터베이스 관리 권한이 있는 사용자가 부여할 수 있는 권한      

#### 시스템 권한 부여
```sql
GRANT [시스템 권한] TO [사용자 이름/롤(Role)이름/PUBLIC]

[WITH ADMIN OPTION]
```

- [시스템 권한]    
오라클 데이터베이스에서 제공하는 시스템 권한을 지정한다. 한번에 여러 종류의 권한을 부여하려면 쉼표로 구분하여 권한 이름을
여러개 명시해주면 된다.    
- [사용자 이름/롤(Role)이름/PUBLIC]     
권한을 부여하려는 대상을 지정한다. PUBLIC은 현재 오라클 데이터베이스의 모든 사용자에게 권한을 부여하겠다는 의미이다.       
- [WITH ADMIN OPTION]     
현재 GRANT 문을 통해 부여받은 권한을 다른 사용자에게 부여할 수 있는 권한도 함께 부여받는다.     

#### 시스템 권한 취소
```sql
REVOKE [시스템 권한] FORM [사용자 이름/롤(Role)이름/PUBLIC];
```

#### 객체 권한이란?
특정 사용자가 생성한 테이블, 인덱스, 뷰, 시퀀스 등과 관련된 권한이다.     

#### 객체 권한 부여
```sql
GRANT [객체 권한/ALL PRIVILEGES]
  ON [스키마.객체 이름]
  TO [사용자 이름/롤(Role)이름/PUBLIC]
  [WITH ADMIN OPTION]
```

#### 객체 권한 취소
```sql
REVOKE [객체 권한/ALL PRIVILEGES]
    ON [스키마.객체 이름]
  FROM [사용자 이름/롤(Role)이름/PUBLIC]
[CASCADING CONSTRAINTS/FORCE]
```

### 15-3 롤 관리
### 롤이란?
- 사용자는 데이터베이스에서 어떤 작업을 진행하기 위해 해당 작업과 관련된 권한을 반드시 부여받아야 하는데,      
신규 생성 사용자의 경우 아무런 권한이 없어 다양한 권한을 일일이 부여해줘야 한다.     
- 이런 불편한 점을 해결하기 위해 롤을 사용한다. 롤은 여러 종류의 권한을 묶어 놓은 그룹을 뜻한다.      
- 롤을 통해 권한 관리 효율을 높일 수 있다.    
- 롤은 오라클 데이터베이스 설치시 기본으로 제공되는 **사전 정의된 롤** 과 **사용자 정의 롤** 로 나뉜다.     

#### 사전 정의된 롤
- CONNECT 롤    
사용자가 데이터베이스 접속에 필요한 CREATE SESSION 권한을 가지고 있다.    
- RESOURCE 롤       
사용자가 테이블, 시퀀스를 비롯한 여러 객체를 생성할 수 있는 기본 시스템 권한을 묶어 놓은 롤이다.         
- DBA 롤     
데이터베이스를 관리하는 시스템 권한을 대부분 가지고 있다.    

#### 사용자 정의 롤     
사용자 롤을 생성하는 절차    
1. CREATE ROLE문으로 롤을 생성한다.    
2. GRANT 명령어로 생성한 롤에 권한을 포함시킨다.      
3. GRANT 명령어로 권한이 포함된 롤을 특정 사용자에게 부여한다.    
4. REVOKE 명령어로 롤을 취소시킨다.     

#### 잊기 전에 한번더!
```sql
-- Q1
-- 1) 
CREATE USER PREV_HW
IDENTIFIED BY ORCL;

-- 2)
GRANT CREATE SESSION TO PREV_HW;

-- Q2
GRANT SELECT ON EMP TO PREV_HW;
GRANT SELECT ON DEPT TO PREV_HW;
GRANT SELECT ON SALGRADE TO PREV_HW;

-- Q3
REVOKE SELECT ON SALGRADE FROM PREV_HW;
```
