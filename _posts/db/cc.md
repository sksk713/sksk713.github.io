<!-- ---
layout: post
title: "데이터 타입에 알맞은 데이터 만들기"
author: "SangKyenog Lee"
tags: Database
---

< 모두를 위한 PostgreSQL >을 공부하고 정리 했습니다. 

---

## 데이터 타입
- 데이터 타입은 주로 대문자로 표기
- 변수명은 소문자로 표기

### 숫자형
#### INTEGER
- 일반적으로 많이 사용되며, 숫자 길이 제한 불가

#### NUMERIC(p, q)
- 소수점자리 표시 가능, DECIMAL과 같음
- p는 전체 자릿수, q는 소수점 자릿수를 의미하며
    - 예시로 NUMERIC(4, 3)은 0.001부터 9.999까지 입력이 가능하다.

- NUMERIC(p)는 입력 받는 숫자의 길이를 제한할때 사용한다,

#### FLOAT
- 부동 소수점 사용, REAR 또는 DOUBLE PRECISION으로 인식

#### SERIAL
- INTEGER 기본값으로 1씩 추가되며 값이 자동 생성, 프라이머 키 데이터 타입으로 주로 사용

### 화폐형
- 화폐형은 금액을 저장하는 데이터 타입이고, 분수의 형태로 금액을 저장

### 문자형
- VARCHAR(n): 문자열 길이가 가변적이며, 문자열 길이가 n이하의 문자 x라면 x만큼 저장
- CHAR(n): n만큼의 문자열이 저장되어 x가 n보다 길이가 짧다면 공백으로 채운다.
- TEXT: 길이에 상관없이 모든 문자열을 저장
    - = n이 주어지지 않은 `VARCHAR`와 같다.

#### CHAR보다 VARCHAR 성능이 더 좋다!!

### 날짜 및 시간
#### TIMESTAMP
- 날짜와 시간정보 모두 나타낸다.
    - 시간대 정보를 반영하지 않은 TIMESTAMP WITHOUT TIME ZONE(세계 표준시)
    - 시간대 정보를 반영한 TIMESTAMP WITH TIME ZONE(= TIMESTAMPZ)
#### DATE
- 날짜 정보만 저장하는 자료형
#### TIME
- 시간 정보만 저장하는 자료형
    - TIME WITHOUT TIME ZONE
    - TIME WITH TIME ZONE

### 불리언형

|데이터타입|설명|
|--|--|
|TRUE|True, yes, on, 1|
|FALSE|False, no, off, 0|
|Null|알 수 없는 정보 or 불확실|
{:.mbtablestyle}

### 배열형
```
phone_num INTEGER[]
```

### JSON형
- 서버와 웹 애플리케이션 간에 데이터를 주고받을 때 사용
- JSON은 `키-밸류`의 쌍으로 구성된 JSON `오브젝트`와 배열과 비슷한 JSON `배열`로 나뉜다.

#### JSON 데이터 타입
1. JSON
- 장점
    - 입력한 텍스트의 정확한 사본 생성
- 단점
    - 처리 속도가 느림
2. JSONB: 텍스트를 이진 형태로 분해 후 저장
- 장점
    - 처리 속도가 비교적 빠름
- 단점
    - 데이터 저장 속도가 비교적 느림

## 데이터 타입 변경
- CAST 연산잔
- CAST 형 연산자

### CAST 연산자
```
CAST (표현식 AS 바꿀 데이터 타입)
```
```
SELECT CAST ('3000' AS INTEGER);
```

### CAST 형 연산자
- `::` 라는 기호를 사용하며 `값::바꿀 데이터 타입`

## 데이터의 값 제한
### 무결성(Integrity)
- 데이터베이스 내에 정확하고 유효한 데이터만을 유지시키는 속성
    - 불필요한 데이터는 제거한다.
    - 합칠 수 있는 데이터는 합친다.
- CRUD할때 데이터 값을 일관되고 정확하게 유지하자는 특성

### 무결성의 제약 조건
1. 개체 무결성(Entity integrity)
- 개체 무결성은 모든 테이블이 프라이머리 키를 가져야 하며, `프라이머리 키`로 선택된 컬럼은 NULL값을 허용하지 않는다.

2. 참조 무결성(Referential integrity)
- `외래 키(foreign key)` 값이 null 값이거나 참조된 테이블의 기본 키값과 동일해야 한다.

3. 범위 무결성(Domain integrity)
- 사용자가 정의한 도메인 내에서 관계형 데이터베이스의 모든 열을 정의한다.
#### 도메인
```
CREATE DOMAIN phoneint AS integer CHECK (VALUE > 0 AND VALUE < 9);
```
- 다음 도메인에서 전화번호는 0~9의 숫자만 입력되야 됨.

## 컬럼 값 제한
1. NOT NULL 제약조건
2. UNIQUE 제약조건
3. primary key
4. foreign key
5. CHECK 제약조건

### NOT NULL
- 빈 값을 허용하지 않는 조건
- 반드시 채워야 함

### UNIQUE
- 해당 컬럼 값은 테이블 내에서 유일한 값을 가져야 한다.

### 프라이머리 키(Primary Key)
- 프라이머리 키의 컬럼 값은 서로 달라야하며, 빈 값을 허용하지 않아야 한다.
- 데이터 타입으로 보통 SERIAL을 사용한다.

### 외래 키
- 자식이 부모의 행동을 참조하듯이, 참조 관계에서 참조되는 테이블은 먼저 생성되어 있어야 하며, 해당 테이블을 부모 테이블이라고 한다.

#### 조건
1. 부모 테이블이 자식 테이블보다 먼저 생성되어야 한다.
2. 부모 테이블은 자식 테이블과 같은 데이터 타입을 가져야 한다.
3. 부모 테이블에서 참조 된 컬럼의 값만 자식 테이블에서 입력 가능하다.
4. 참조되는 컬럼은 모두 프라이머리 키이거나 UNIQUE 제약조건 형식이어야 한다.

#### 예시
![16](/assets/postgreimage/db16.png)

```sql
DROP TABLE IF EXISTS teacher;
DROP TABLE IF EXISTS subject;

CREATE TABLE subject (
    subj_id NUMERIC(5) NOT NULL PRIMARY KEY,
    subj_name VARCHAR(60) NOT NULL
);

INSERT INTO subject
VALUES (00001, 'mathmatics'),
        (00002, 'science'),
        (00003, 'programming')

CREATE TABLE teacher (
    teac_id NUMERIC(5) NOT NULL PRIMARY KEY,
    teac_name VARCHAR(20) NOT NULL,
    subj_id NUMERIC(5) REFERENCES subject,
    teac_certifi_date DATE
);

INSERT INTO teacher values (00011, '정선생',00001, '2017-03-11'); --성공
INSERT INTO teacher values (00021, '홍선생',00001, '2017-04-12'); --성공
INSERT INTO teacher values (00031, '박선생',00001, '2017-04-13'); -- 성공
INSERT INTO teacher values (00041, '한선생',00001, '2018-05-20'); --실패

-- 만약 여러개의 외래 키를 참조한다면 다음과 같다.
CREATE TABLE teacher (
    teac_id NUMERIC(5) NOT NULL PRIMARY KEY,
    teac_name VARCHAR(20) NOT NULL,
    subj_code NUMERIC(5) NOT NULL,
    subj_name VARCHAR(60) NOT NULL,
    teac_certifi_date DATE 
    FOREIGN KEY (subj_code, subj_name) REFERENCES subject (subj_id, subj_name)
);

```

## 참조된 테이블 삭제
- 원칙적으로 부모 테이블은 참조 중인 자식 테이블보다 먼저 지울 수 없다.

### ON DELETE
- 지우면 안되는 경우
    1. ON DELETE NO ACTION
    2. ON DELETE RESTRICT
- 지워야 하는 경우
    3. ON DELETE SET NULL
    4. ON DELETE CASCADE
    5. ON DELETE SET DEFAULT

#### 125페이지 참조

## CHECK
- CHECK는 제약 조건을 만들때 사용한다.

```sql
CREATE TABLE order_info (
    order_qty INTEGER CHECK (order_qty > 0)
);
```

## ALTER 이어서 나중에 -->