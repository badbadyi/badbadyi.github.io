---

title: SpringBoot中使用PostgreSQL
categories: [计算机, 速记]
tags: [速记, PostgreSQL]
authors: [badbadyi]

---

## Maven依赖

```
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

## YML配置

```yaml
spring:
  datasource:
    username: username
    password: password
    url: jdbc:postgresql://localhost:5432/mydatabase
  jpa:
    database: postgresql
```

