# 🧳 데이터 베이스 사용하기

[목표]

- In-Memory 데이터 베이스(H2) 사용하기
- MariaDB 연결하기
- Spring-Data-JPA 사용하기

<br>

### In-Memory 데이터 베이스 사용하기

> - 스프링 부트에서 지원하는 In-Memory 데이터베이스 : H2, HSQL, Derby
> - 그 중 H2 의존성 추가해서 사용해보기

<br>

#### 1. 부트 프로젝트 파일 설정

##### >>> 의존성 추가하기

[pom.xml] : Spring-boot-starter-jdbc 의존성 추가 => 설정이 필요한 bean 자동 설정

```xml
<dependency>
 <groupId>com.h2database</groupId>
 <artifactId>h2</artifactId>
 <scope>runtime</scope>
</dependency>
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

<br>

##### >>> 코드 작성

[DBRunner.java] : Connectionr과 DataSource로 연결정보 받아오기

```java
@Component
@Order(1)
public class DBRunner implements ApplicationRunner{

	@Autowired
	private DataSource dataSource;

	private Logger logger = LoggerFactory.getLogger(DBRunner.class);


	@Override
	public void run(ApplicationArguments args) throws Exception {

		logger.info("=====>> connection ===========");
		Connection connection = dataSource.getConnection();
		DatabaseMetaData dbMeta = connection.getMetaData();
		logger.debug("DB URL : " + dbMeta.getURL());
		logger.debug("DB Username : " +dbMeta.getUserName());
		logger.info("=====>> end =============");
	}
}
```

[콘솔]

    =====>> connection ===========
    DB URL : jdbc:h2:mem:testdb
    DB Username : SA
    =====>> end =============

<br>

#### 2. 웹 브라우저 : H2 웹페이지에서 DB 관리 가능

localhost:8087/h2-console로 접속

![](./imgs/2.h2.png)

JDBC URL 콘솔에 나온 값으로 변경

![](./imgs/2.h3.png)

Connect후 화면

![](./imgs/2.h4.png)

<br><br>

### MariaDB 사용하기

> - **스프링 부트는 기본적으로 제공하는 DBCP** (DataBase Connection Pooling)
>
> 1. HikariDP ( - spring.datasource.hikrai.\*)
> 2. Tomcat CP ( - spring.datasource.tomcat.\*)
> 3. Commons DBCP2 ( - spring.datasource.dbcp2)
>
> - MySQL 커넥터 의존성과 MySQL Datasource 설정으로 MariaDB에 연결할 수 있음

<br>

##### >>> 의존성 추가하기

[pom.xml] : MySQL 커텍터 의존성 추가

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.13</version>
</dependency>
```

##### >>> 환경 설정

[application.properties] MySQL DataSource 설정

```xml
#mysql 설정
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/spring_db?useUnicode=true&charaterEncoding=utf-8&useSSL=false&serverTimezone=UTC
spring.datasource.username=spring
spring.datasource.password=spring
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
```

[터미널로 Maria DB에 접속 및 데이터 추가] : MySQL Database 생성

```sql
mysql -u root –p
show databases;
use mysql;
create user spring@localhost identified by 'spring';
grant all on *.* to spring@localhost;
flush privileges;
exit;
mysql -u spring -p
create database spring_db;
show databases;
use spring_db;
```

![](./imgs/2.terminalPic.png)

<br><br>

### Spring-Data-JPA 사용하기

#### ORM과 JPA 란?

> - **ORM**(Object-Relational Mapping) : 관계형 DB테이블을 객체지향적으로 사용하기 위해 객체와 관계를 연결해줌 (DB테이블 자체를 객체로 사용)
> - **JPA**(Java Persistance API) : ORM 표준기술로 데이터베이스 정보를 객체 지향으로 쉽게 적용할 수 있게 도와줌
> - 그 중, 스프링부트는 Hibernate로 구현

![](./imgs/2.orm_jpa.png)

<br>

#### Spring-Data-JPA의 특징

> - JPA를 쉽게 사용하기 위해 스프링에서 제공하는 프레임워크
> - Repository Bean을 자동생성
> - 쿼리 메소드 자동구현
> - @EnalbeJpaRepositories (스프링부트가 자동으로 설정해줌)

<br>

##### >>> 의존성 추가하기

[pom.xml] : Spring-Data-JPA 의존성 추가

```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

<br>

##### >>> 코드 작성

[사용 어노테이션 먼저 알기]

| 어노테이션      | 기능                                                                                                          |
| --------------- | ------------------------------------------------------------------------------------------------------------- |
| @Entity         | - 엔티티 클래스임을 지정하며 **DB 테이블**과 매핑하는 객체를 나타냄                                           |
| @Id             | - 엔티티의 **기본키**를 나타내는 어노테이션입니다.                                                            |
| @GeneratedValue | - 주 키의 값을 **자동 생성**하기 위해 명시하는 데 사용 <br> -자동 생성 전략 : AUTO, IDENTITY, SEQUENCE, TABLE |

**앤티티(Entity)란** 데이터베이스에서 표현하려고 하는 유형, 무형의 객체로서 서로 구별되는 것을 뜻함. 이 객체들은 DB 상에서는 보통 table로서 나타남

**객체 <=> DB 매핑**

    - 클래스(엔티티) <=> 테이블
    - 객체 <=> Row (행)
    - 변수 <=> Column (열)

<br>

[Account.java] : DB Account 테이블 생성하기

```java
@Entity
public class Account {
	@Id @GeneratedValue(strategy = GenerationType.IDENTITY)
	//primary key만들기
	private Long id;

	@Column(unique = true)
	private String username;

	private String password;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	@Override
	public String toString() {
		return "Account [id=" + id + ", username=" + username + ", password=" + password + "]";
	}
}
```

<br>

[AccountRepository.java] : Repository 인터페이스 작성

> - AccountRepository의 구현클래스를 따로 작성하지 않아도 Spring-Data-JPA가 자동적으로 해당 문자열
>   username에 대한 인수를 받아 자동적으로 DB Table에 매핑
> - JpaRepository<엔티티 클래스, 프라이머리 키 타입>
> - JpaRepository의 상위 객체 CrudRepository가 (save, findOne, findAll) 처리

```java
public interface AccountRepository extends JpaRepository<Account, Long>{

	//findBy(column명)
	Account findByUsername(String username);
}
```

[applicaion.properties] : JPA로 데이터베이스를 자동 초기화하도록 설정

```xml
#SQL 설정
spring.jpa.hibernate.ddl-auto=create
spring.jpa.show-sql=true
```

<br>

[jpa 설정옵션]

**spring.jpa.hibernate.ddl-auto=** create|create-drop|update|validate

**1. create**

> JPA가 DB와 상호작용할 때 기존에 있던 스키마(테이블)을 삭제하고 새로 만드는 것을 뜻함

**2. create-drop**

> JPA 종료 시점에 기존에 있었던 테이블을 삭제

**3. update**

> 기존 스키마는 유지하고, 새로운 것만 추가하고, 기존의 데이터도 유지한다. 변경된 부분만 반영함
> 주로 개발 할 때 적합

**4.validate**

> 엔티티와 테이블이 정상 매핑 되어 있는지를 검증합니다.

**spring.jpa.show-sql=true**

> JPA가 생성한 SQL문을 보여줄 지에 대한 여부를 알려줌

<br>

[src/test/java/AppRepoTest.java] : 실제로 쿼리가 정상적으로 동작하는지 jUnit 확인

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class AppRepoTest {
	@Autowired
	private AccountRepository repository;

	@Test
	public void account() throws Exception {
		System.out.println(repository.getClass().getName());
		//1. Account 객체 생성 -> 등록
		Account account = new Account();
		account.setUsername("Spring2");
		account.setPassword("1234");

		Account addAccount = repository.save(account);
		System.out.println(addAccount.getId() + " " + addAccount.getUsername());
	}
}
```

[jUnit 실행결과] : 정상 작동

![](./imgs/2.junit.png)

[table확인] : 추가한 데이터 확인

![](./imgs/2.teminal2.png)
