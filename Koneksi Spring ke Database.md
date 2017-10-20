# Koneksi Spring ke Database
- Menambahkan Dependency Spring Data JPA ke Project
- Konfigurasi ke Oracle Database
- Memunculkan SQL Pada Console

## Menambahkan Dependency Spring Data JPA ke Project
Pada pom.xml, tambahkan dependency berikut ini:
``` xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

## Konfigurasi ke Oracle Database
Pada application.properties, tambahkan konfigurasi berikut ini:
``` properties
spring.datasource.url=jdbc:oracle:thin:@localhost:1521:{DB_NAME}
spring.datasource.driver-class-name=oracle.jdbc.driver.OracleDriver
spring.datasource.username={USERNAME}
spring.datasource.password={PASSWORD}
spring.jpa.database-platform=org.hibernate.dialect.Oracle12cDialect atau org.hibernate.dialect.Oracle10gDialect
```

{} = parameter yang harus diisi sesuai dengan konfigurasi database Oracle

## Memunculkan SQL Pada Console
Pada application.properties, tambahkan konfigurasi berikut ini:
``` properties
spring.jpa.show-sql=true
```

## Memformat SQL Pada Console
Pada application.properties, tambahkan konfigurasi berikut ini:
``` properties
spring.jpa.properties.hibernate.format_sql=true
```
