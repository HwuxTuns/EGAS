# EGAS - Hướng dẫn khởi tạo dự án (Phase 0)

## Bước 1: Tạo Spring Boot Project

Truy cập [start.spring.io](https://start.spring.io) hoặc dùng IDE:

| Cấu hình       | Giá trị                                |
|----------------|----------------------------------------|
| Project        | Maven                                  |
| Language        | Java 17+                              |
| Spring Boot    | 3.x (latest stable)                   |
| Group          | `com.egas`                             |
| Artifact       | `egas`                                 |
| Package name   | `com.egas`                             |

### Dependencies cần thêm:
```xml
<!-- pom.xml -->
<dependencies>
    <!-- Core -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- Database -->
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- Utility -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>

    <!-- JWT -->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-api</artifactId>
        <version>0.12.3</version>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-impl</artifactId>
        <version>0.12.3</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-jackson</artifactId>
        <version>0.12.3</version>
        <scope>runtime</scope>
    </dependency>

    <!-- Test -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## Bước 2: Cấu hình application.yml

```yaml
# src/main/resources/application.yml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/egas_db?createDatabaseIfNotExist=true
    username: root
    password: root123
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
        format_sql: true

  thymeleaf:
    cache: false

# Custom configs
app:
  jwt:
    secret: your-secret-key-here-change-in-production
    expiration: 86400000  # 24h in ms
  github:
    token: your-github-token
    api-url: https://api.github.com
  gemini:
    api-key: your-gemini-api-key
```

## Bước 3: Docker Compose cho MySQL

```yaml
# docker-compose.yml
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    container_name: egas-mysql
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: egas_db
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

Chạy: `docker-compose up -d`

## Bước 4: Tạo cấu trúc packages

Tạo các package (thư mục) sau trong `src/main/java/com/egas/`:

```
com.egas
├── config/
├── controller/
├── dto/
│   ├── request/
│   └── response/
├── entity/
├── exception/
├── repository/
├── security/
├── service/
│   ├── interfaces/
│   ├── impl/
│   ├── strategy/
│   └── factory/
└── util/
```

> 💡 Trong Java, mỗi package cần ít nhất 1 file.
> Ở bước tiếp theo, mình sẽ tạo các class stub cho từng package.
