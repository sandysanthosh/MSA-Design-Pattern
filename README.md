Spring Boot leverages various design patterns to provide a robust and scalable framework for developing Java applications. Here are some of the key design patterns used in Spring Boot:

1. **Singleton Pattern**:
   - **Usage**: Ensures a class has only one instance and provides a global point of access to it.
   - **Spring Example**: Spring beans are by default singletons, managed by the Spring container.

2. **Factory Pattern**:
   - **Usage**: Creates objects without specifying the exact class of the object that will be created.
   - **Spring Example**: The `FactoryBean` interface in Spring provides a way to create objects through a factory.

3. **Proxy Pattern**:
   - **Usage**: Provides a surrogate or placeholder for another object to control access to it.
   - **Spring Example**: Used extensively in AOP (Aspect-Oriented Programming) to create proxies for objects to implement cross-cutting concerns like logging, transaction management, etc.

4. **Template Method Pattern**:
   - **Usage**: Defines the skeleton of an algorithm in a method, deferring some steps to subclasses.
   - **Spring Example**: The `JdbcTemplate` and `RestTemplate` classes provide templates for database and REST operations, respectively.

5. **Dependency Injection Pattern**:
   - **Usage**: Allows a class to be independent of its dependencies, which are provided by an external source rather than the class itself.
   - **Spring Example**: Core to Spring, where the container manages the lifecycle and dependencies of beans.

6. **MVC (Model-View-Controller) Pattern**:
   - **Usage**: Separates an application into three main components: Model (data), View (UI), and Controller (business logic).
   - **Spring Example**: Spring MVC framework implements this pattern to build web applications.

7. **Observer Pattern**:
   - **Usage**: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
   - **Spring Example**: Spring’s event-driven architecture, where beans can listen for and react to events.

8. **Decorator Pattern**:
   - **Usage**: Adds behavior to objects dynamically by wrapping them in an object of a decorator class.
   - **Spring Example**: Used in Spring AOP to add additional behavior to methods.

9. **Builder Pattern**:
   - **Usage**: Separates the construction of a complex object from its representation.
   - **Spring Example**: Often used in the configuration of beans and constructing complex objects like `RestTemplate` or `WebClient` instances.

10. **Strategy Pattern**:
    - **Usage**: Defines a family of algorithms, encapsulates each one, and makes them interchangeable.
    - **Spring Example**: The `TaskExecutor` and `DataSource` interfaces allow for different implementations to be plugged in.

### Practical Examples in Spring Boot

1. **Singleton Pattern Example**:
   ```java
   @Service
   public class MyService {
       // Singleton service bean
   }
   ```

2. **Factory Pattern Example**:
   ```java
   @Configuration
   public class MyConfig {
       @Bean
       public MyService myService() {
           return new MyServiceImpl();
       }
   }
   ```

3. **Proxy Pattern Example**:
   ```java
   @Aspect
   public class LoggingAspect {
       @Before("execution(* com.example.service.*.*(..))")
       public void logBefore(JoinPoint joinPoint) {
           System.out.println("Method called: " + joinPoint.getSignature().getName());
       }
   }
   ```

4. **Template Method Pattern Example**:
   ```java
   @Autowired
   private JdbcTemplate jdbcTemplate;

   public void queryDatabase() {
       String sql = "SELECT * FROM users";
       jdbcTemplate.query(sql, (rs, rowNum) -> new User(rs.getString("name")));
   }
   ```

5. **Dependency Injection Pattern Example**:
   ```java
   @Service
   public class UserService {
       private final UserRepository userRepository;

       @Autowired
       public UserService(UserRepository userRepository) {
           this.userRepository = userRepository;
       }
   }
   ```

By understanding and applying these design patterns, you can leverage Spring Boot’s capabilities to build robust, maintainable, and scalable applications.
