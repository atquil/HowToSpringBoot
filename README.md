# Transcation Management

Transaction management is an essential aspect of any enterprise application. It ensures **data integrity** and **consistency** in the event of an application failure, and it is responsible for managing the transaction's lifecycle, including its beginning, ending, and rollback.

Spring Boot, transaction management can be achieved using the Spring Framework's **Transaction Management API** or **Hibernate's Transaction Management API**.

**Hibernate** is a popular **object-relational mapping (OR**M) framework used to map Java objects to relational database tables. It provides a robust set of transaction management APIs that can be used in conjunction with Spring Boot.

<br>
<details>
  <summary>
    <span style="font-size: x-large;">What is ACID in transaction?</span>
  </summary>
  <p>
ACID stands for Atomicity, Consistency, Isolation, and Durability, which are a set of properties that describe the characteristics of a transaction in database management systems. ACID properties ensure that transactions are processed reliably in a consistent and predictable manner, and they are the fundamental building blocks of transaction management.

1. **Atomicity**: **All or Nothing** principle. Either all operations performed when committed or roll back if error occurs
2. **Consistency**: It ensures the transaction takes the system to consistent state
3. **Isolation**: Changes performed are **not visible to any other transaction** until successfully commit
4. **Durability** : It ensures that **committed changes get persisted**
  </p>
</details>
<br>
<details>
  <summary>
    <span style="font-size: x-large;">Different type of @Transcation</span>
  </summary>
  <p>
ACID stands for Atomicity, Consistency, Isolation, and Durability, which are a set of properties that describe the characteristics of a transaction in database management systems. ACID properties ensure that transactions are processed reliably in a consistent and predictable manner, and they are the fundamental building blocks of transaction management.

1. **Atomicity**: **All or Nothing** principle. Either all operations performed when committed or roll back if error occurs
2. **Consistency**: It ensures the transaction takes the system to consistent state
3. **Isolation**: Changes performed are **not visible to any other transaction** until successfully commit
4. **Durability** : It ensures that **committed changes get persisted**
  </p>
</details>
<br>
<details>
  <summary>
    <span style="font-size: x-large;">How to implement</span>
  </summary>
  <p>

**Step 1: Add Dependency**

To use Hibernate and Spring Boot together, we need to add the necessary dependencies to our project's

pom.xml file
```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
   <groupId>org.hibernate</groupId>
   <artifactId>hibernate-core</artifactId>
</dependency>

```
or gradle
```properties
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.hibernate:hibernate-core'
}

```
**Step 2: Configure the Database**

application.properties
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/db_name
spring.datasource.username=username
spring.datasource.password=password
#Ddl-auto should be avoided in prod, as database changes should be performed saperately
spring.jpa.hibernate.ddl-auto=create 
spring.jpa.hibernate.show-sql=true

```
or
application.yml (prefered)
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db_name
    username: username
    password: password
  jpa:
    hibernate:
      ddl-auto: create
    show-sql: true

```

**Step 3: Create a Transaction Service**

We can use the @Transactional annotation to mark the methods or class  that need to be executed within a transaction.
Note that , if the class and method both has @Transactional then method has the priority

```java
@Service
//@Transactional
public class EmployeeService {
 
    @Autowired
    private EmployeeRepository employeeRepository;

    @Transactional
    public Employee saveEmployee(Employee employee) {
        return employeeRepository.save(employee);
    }

    @Transactional
    public List<Employee> getAllEmployees() {
        return employeeRepository.findAll();
    }
}

```

**Step 4: Create the Repository **
We need to create a repository that will handle database operations. We can use the JpaRepository interface provided by Spring Boot to handle database operations.

```java
@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Long> {
}

```

**Step 5: Create a controller, to save/retrieve the data**
We can now use the transactional service in our controller to handle HTTP requests.

```java
@RestController
@RequestMapping("/employees")
public class EmployeeController {
 
    @Autowired
    private EmployeeService employeeService;
 
    @PostMapping
    public Employee createEmployee(@RequestBody Employee employee) {
        return employeeService.saveEmployee(employee);
    }
 
    @GetMapping
    public List<Employee> getAllEmployees() {
        return employeeService.getAllEmployees();
    }
}

```
</p>
</details>
<br>
<details>
  <summary>
    <span style="font-size: x-large;">@Transaction types</span>
  </summary>
  <p>
In Spring Boot, the @Transactional annotation can be used to control the transactional behavior of methods. The annotation provides several options for defining the transaction scope and propagation. Here are the different types of transactional behavior that can be defined using the @Transactional annotation:

| Transaction Type | Description                                                                                                                                                                                                              |  
|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| REQUIRED         | The method must run within a transaction context. If an existing transaction context exists, the method uses that transaction. Otherwise, a new transaction context is created.                                          |
| REQUIRES_NEW     | The method always runs within a new transaction context, regardless of whether an existing transaction context exists. If a transaction is already in progress when the method is called, that transaction is suspended. |
| MANDATORY        |                                                                                                                                                                                                                          |

  </p>
</details>