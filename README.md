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


Sure! Here are examples of each design pattern applied in Spring Boot:

### 1. Singleton Pattern

In Spring Boot, all beans are singletons by default unless specified otherwise.

```java
import org.springframework.stereotype.Service;

@Service
public class SingletonService {
    // Singleton service bean
}
```

### 2. Factory Pattern

Using `FactoryBean` to create beans:

```java
import org.springframework.beans.factory.FactoryBean;
import org.springframework.stereotype.Component;

@Component
public class MyFactoryBean implements FactoryBean<MyService> {

    @Override
    public MyService getObject() throws Exception {
        return new MyServiceImpl();
    }

    @Override
    public Class<?> getObjectType() {
        return MyService.class;
    }

    @Override
    public boolean isSingleton() {
        return true;
    }
}
```

### 3. Proxy Pattern

Using Spring AOP to create a proxy for logging:

```java
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Method called: " + joinPoint.getSignature().getName());
    }
}
```

### 4. Template Method Pattern

Using `JdbcTemplate` for database operations:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class UserRepository {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    public List<User> findAll() {
        String sql = "SELECT * FROM users";
        return jdbcTemplate.query(sql, (rs, rowNum) -> new User(rs.getString("name")));
    }
}
```

### 5. Dependency Injection Pattern

Injecting a repository into a service:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}
```

### 6. MVC Pattern

Defining a controller, service, and repository in Spring MVC:

```java
// Controller
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/users")
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
}

// Service
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}

// Repository
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

### 7. Observer Pattern

Creating an event and a listener:

```java
// Event
import org.springframework.context.ApplicationEvent;

public class UserCreatedEvent extends ApplicationEvent {
    private final User user;

    public UserCreatedEvent(Object source, User user) {
        super(source);
        this.user = user;
    }

    public User getUser() {
        return user;
    }
}

// Listener
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

@Component
public class UserCreatedListener {

    @EventListener
    public void handleUserCreatedEvent(UserCreatedEvent event) {
        System.out.println("User created: " + event.getUser().getName());
    }
}

// Publishing the event
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private ApplicationEventPublisher eventPublisher;

    public void createUser(User user) {
        userRepository.save(user);
        eventPublisher.publishEvent(new UserCreatedEvent(this, user));
    }
}
```

### 8. Decorator Pattern

Creating a decorator for a service:

```java
// Service Interface
public interface UserService {
    User getUserById(Long id);
}

// Service Implementation
@Service
public class UserServiceImpl implements UserService {

    @Override
    public User getUserById(Long id) {
        // Fetch user from database
        return new User(id, "John Doe");
    }
}

// Decorator
@Service
public class UserServiceDecorator implements UserService {

    @Autowired
    private UserServiceImpl userService;

    @Override
    public User getUserById(Long id) {
        User user = userService.getUserById(id);
        // Add additional behavior
        System.out.println("User fetched: " + user.getName());
        return user;
    }
}
```

### 9. Builder Pattern

Building a `RestTemplate` with custom settings:

```java
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class AppConfig {

    @Bean
    public RestTemplate restTemplate(RestTemplateBuilder builder) {
        return builder
                .setConnectTimeout(Duration.ofMillis(5000))
                .setReadTimeout(Duration.ofMillis(5000))
                .build();
    }
}
```

### 10. Strategy Pattern

Using different `TaskExecutor` implementations:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.task.TaskExecutor;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration
public class TaskExecutorConfig {

    @Bean
    public TaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(25);
        executor.initialize();
        return executor;
    }
}
```

These examples illustrate how various design patterns are applied in Spring Boot to create a well-structured and maintainable codebase.
