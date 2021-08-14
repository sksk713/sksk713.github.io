---
layout: post
title: "SpringBoot PostgreSQL 연동"
author: "SangKyenog Lee"
tags: Spring
---

## Gradle 설정
- 실행 시점에만 필요하므로 `runtimeOnly`를 사용

```java
dependencies {
	runtimeOnly 'org.postgresql:postgresql'
}
```

## application.properties 설정
```
spring.datasource.username=postgres
spring.datasource.password=암호입력
spring.datasource.url=jdbc:postgresql://localhost:5433/postgres
```

- PostgreSQL 디폴트 port가 5432인데 설치할때, 5433으로 해버림

## psql

![](/assets/etc/01.png)
- Enter 눌러서 스킵하고 설정해준 암호만 쳐주면 된다.


## PgAdmin 4

1. PgAdmin 4 접속한다.
2. 새로운 서버 추가를 하고 General 탭에서 Name엔 원하는 서버 이름을 넣는다.
3. Connection 탭에서 Hostname엔 `Localhost`를 넣고 암호를 누르고 save를 누르면 설정 완료