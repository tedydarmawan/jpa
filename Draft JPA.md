# Outline
- Object Relational Impedence Mismatch
- Pengenalan JPA
- Membuat Projek JPA menggunakan Spring Initialzr

## Object Relational Impedence Mismatch
Object-relational impedance mismatch adalah masalah konseptual dan teknis yang sering dihadapi ketika bahasa pemrograman berorientasi objek
harus menggunakan/mengimplementasikan relational database dikarenakan objek atau kelas harus termapping ke tabel pada database.

- Java -> Kelas -> Objek
- Database -> Relational Database -> Tabel
- Objek != Tabel -> Object-relational impedance mismatch

__Contoh object-relational impedance mismatch ketika membuat objek atau tabel untuk suatu entitas__

Tabel:
``` sql
CREATE TABLE task(
  id NUMBER,
  description VARCHAR(255),
  is_done NUMBER,
  target_date DATE,
  PRIMARY KEY(id)
)
```

Kelas/Objek:
``` java
public class Task{
  private int id;
  private String desc;
  private Date targetDate;
  private boolean isDone;
}
```

__Contoh object-relational impedance mismatch ketika membuat relasi pada suatu tabel dibandingkan dengan relasi diantara kelas__

Tabel:
``` sql
CREATE TABLE employee(
  employee_id number,
  -- Kolom lainnya
)

CREATE TABLE task(
  id number,
  -- Kolom lainnya
)

CREATE TABLE employee_task(
  employee_id number,
  id number
)
```

Kelas:
``` java
public class Employee{
  
  private List<Task> tasks;
  
  // kode lainnya
  
}

public class Task{

  private List<Employee> employees;
  
  // kode lainnya

}
```

## Step 2 - Pengenalan JPA
JPA (Java Persistence API) muncul untuk menyelesaikan permasalahan object-relational impedance mismatch.

__Contoh penggunaan JPA pada saat pembuatan objek atau tabel untuk suatu entitas__

Tabel:
``` sql
CREATE TABLE task(
  id NUMBER,
  description VARCHAR(255),
  is_done NUMBER,
  target_date DATE,
  PRIMARY KEY(id)
)
```

Kelas/Objek:
``` java
@Entity
@Table(name = "task")
public class Task{
  @Id
  private int id;
  
  @Column(name = "description")
  private String desc;
  
  @Column(name = "target_date")
  private Date targetDate;
  
  @Column(name = "is_done")
  private boolean isDone;
}
```

__Contoh Penggunaan JPA pada saat membuat relasi pada suatu tabel dibandingkan dengan relasi diantara kelas__

Tabel:
``` sql
CREATE TABLE employee(
  employee_id number,
  -- Kolom lainnya
)

CREATE TABLE task(
  id number,
  -- Kolom lainnya
)

CREATE TABLE employee_task(
  employee_id number,
  id number
)
```

Kelas:
``` java
public class Employee{
  @ManyToMany
  private List<Task> tasks;
  
  // kode lainnya
  
}

public class Task{
  @ManyToMany(mappedBy = "tasks")
  private List<Employee> employees;
  
  // kode lainnya

}
```

## Membuat Projek JPA menggunakan Spring Initialzr
- Akses Spring Initialzr atau start.spring.io
- Generate a "Maven Project" with "Java" and Spring Boot "1.5.6" (Stable Release)
- Untuk Project Metadata,
  - Isi Group, misal: com.ss.training
  - Isi Artifact, misal: jpa
- Pilih Dependencies,
  - Web
  - JPA
  - H2
- Klik Generate Project
- Ekstrak file projek
- Import projek yang telah didownload dari Spring Initialzr
  - File > Import > Existing Maven Projects


## Membuat Entity - User
``` java
package com.ss.training.jpa.entity;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
public class User {
	
  @Id
  @GeneratedValue
  private long id;
	
  private String name;

  private String role;

  public User() {
  }
	
  public User(String name, String role) {
    this.name = name;
    this.role = role;
  }

  public long getId() {
    return id;
  }

  public void setId(long id) {
    this.id = id;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public String getRole() {
    return role;
  }

  public void setRole(String role) {
    this.role = role;
  }

  @Override
  public String toString() {
    return "User [id=" + id + ", name=" + name + ", role=" + role + "]";
  }

}
```

## Membuat Service Untuk Mengatur Entity - UserDAOService dan EntityManager
``` java
package com.ss.training.jpa.service;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import javax.transaction.Transactional;

import org.springframework.stereotype.Repository;

import com.ss.training.jpa.entity.User;

@Repository
@Transactional
public class UserDAOService {
	
  @PersistenceContext
  private EntityManager entityManager;
	
  public long insert(User user){
    entityManager.persist(user);
    return user.getId();
  }
	
}
```

## Menggunakan Command Line Runner Untuk Menyimpan User ke Database
Kelas Command Line Runner akan dieksekusi pada saat aplikasi dijalankan.

``` java
package com.ss.training.jpa;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

import com.ss.training.jpa.entity.User;
import com.ss.training.jpa.service.UserDAOService;

@Component
public class UserDAOServiceCommandLineRunner implements CommandLineRunner{
	
  private static final Logger log = LoggerFactory.getLogger(UserDAOServiceCommandLineRunner.class);
	
  @Autowired
  private UserDAOService userDaoService;
	
  @Override
  public void run(String... arg0) throws Exception {
    User user = new User("Tedy", "Admin");
	  userDaoService.insert(user);
	  log.info("User baru telah dibuat: " + user);	
  }
}
```

## Spring Boot dan In Memory Database H2
## Pengenalan Spring Data JPA
## Lebih Lanjut Mengenai JPA Repository: findById dan findAll

# Melihat Data di DB H2
- http://localhost:8080/h2-console
- DB URL: jdbc:h2:mem:testdb

# Spring Data JPA
- Membuat Interface UserRepository extends JpaRepository
- Membuat kelas commandLineRunner untuk UserRepository


