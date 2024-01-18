---

title: PostgreSQL相关问题
categories: [计算机, 数据库]
tags: [PostgreSQL]
authors: [badbadyi]

---

## 数据库相关

### 创建自增字段

需要创建自增为 1 的字段可以直接使用 `SERIAL` 作为数据类型

数据库会创建一个 `表名_字段名_seq` 的序列来实现自动增长

理论上可以通过自己创建 `INTEGER` 字段和对应的序列来达到同样的效果

相应的还有 `SMALLSERIAL` `BIGSERIAL` 数据类型，分别对应 `SMALLINT` 和 `BIGINT`

## Spring Data 相关

### YML配置文件

```yaml
spring:
  datasource:
    username: username
    password: password
    url: jdbc:postgresql://localhost:5432/commondb
    driver-class-name: org.postgresql.Driver
```

## Spring Boot JPA 相关

### YML配置文件

```yaml
spring:
  jpa:
    database: postgresql
```

### DTO类中自增字段

自增本质上是序列，通过如下注解设置

```java
@SequenceGenerator(sequenceName = "account_id_seq", name = "account_id_auto", allocationSize = 1)
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "account_id_auto")
private Integer id;
```

- `@SequenceGenerator` 中的 `sequenceName`

  若字段是 `SERIAL` 字段，则序列名为 `表名_字段名_seq` 

  若是自定义序列，则序列名为自定义的序列名

- `@SequenceGenerator` 中的 `name` 和 `@GeneratedValue` 中的 `generator`

  设置为相同值来绑定

- `@SequenceGenerator` 中的 `allocationSize`

  每次增加的值，默认值是50

  修改为序列设定的值（对于 `SERIAL` 类型，设置为 1 即可）

- `@SequenceGenerator` 中的 `strategy`

  设置为 `GenerationType.SEQUENCE` ，即生成策略设置为序列



