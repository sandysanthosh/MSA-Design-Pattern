In **Spring Boot API development**, certain design patterns are commonly used to build robust, maintainable, and scalable applications. Here are some of the most frequently used patterns:

### 1. **Singleton Pattern**
   - **Usage**: Ensures a class has only one instance and provides a global point of access to it.
   - **Spring Boot**: This is one of the most used patterns in Spring. By default, Spring beans are singleton-scoped, meaning that Spring creates a single instance of a bean and shares it across the entire application.
   - **Example**: Services, repositories, and controllers in Spring Boot are usually singletons.

### 2. **Factory Pattern**
   - **Usage**: Provides an interface for creating instances, with subclasses determining the specific object types created.
   - **Spring Boot**: The `@Bean` annotation in Spring acts like a factory, defining objects created and managed by the Spring IoC container.
   - **Example**: Bean creation methods within `@Configuration` classes use a factory approach to instantiate and configure dependencies.

### 3. **Prototype Pattern**
   - **Usage**: Creates a new instance each time it is needed, instead of reusing a single instance.
   - **Spring Boot**: This pattern is available through the prototype scope. Setting a bean's scope to `@Scope("prototype")` tells Spring to create a new instance every time it is requested.
   - **Example**: Useful for non-thread-safe components or objects that require new data each time.

### 4. **Template Method Pattern**
   - **Usage**: Defines the skeleton of an algorithm, allowing subclasses to fill in specific steps.
   - **Spring Boot**: Used in Spring’s `JdbcTemplate`, `RestTemplate`, and `JpaRepository` classes. These classes provide abstract methods for database or HTTP operations, leaving specific implementations to subclasses.
   - **Example**: `JdbcTemplate` provides general database interaction methods, letting you define specific SQL queries.

### 5. **Proxy Pattern**
   - **Usage**: Provides a placeholder or surrogate for another object to control access or add additional functionality.
   - **Spring Boot**: AOP (Aspect-Oriented Programming) uses proxies in Spring Boot. Spring’s `@Transactional` annotation uses proxies to wrap methods in transaction logic, ensuring automatic commit or rollback.
   - **Example**: Transaction management, security, and caching.

### 6. **Decorator Pattern**
   - **Usage**: Allows behavior to be added to an object dynamically without altering its structure.
   - **Spring Boot**: Commonly used in Spring Boot’s web filters and interceptors, which add behavior (like logging or authentication) to HTTP requests and responses.
   - **Example**: Customizing `HandlerInterceptor` for request logging or `Filter` for adding security checks.

### 7. **Observer Pattern**
   - **Usage**: Defines a one-to-many dependency between objects so that when one object changes state, all dependents are notified.
   - **Spring Boot**: Used in Spring's event-driven architecture, where you can define events and listeners with `@EventListener`.
   - **Example**: Application events like `ApplicationReadyEvent`, `ContextRefreshedEvent`, or custom events to communicate across different parts of the application.

### 8. **Builder Pattern**
   - **Usage**: Provides a flexible solution for creating complex objects step-by-step.
   - **Spring Boot**: Often used in constructing complex objects such as HTTP requests, responses, or configuration settings.
   - **Example**: `UriComponentsBuilder` for creating URIs in a flexible way; `ResponseEntity` builder for constructing HTTP responses.

### 9. **MVC (Model-View-Controller) Pattern**
   - **Usage**: Separates an application into three main components: Model, View, and Controller.
   - **Spring Boot**: A foundational pattern for Spring’s `@Controller` classes that handle HTTP requests, where models are used to carry data, controllers handle requests, and views render HTML (or JSON/XML for APIs).
   - **Example**: All Spring Boot web applications and REST APIs are structured around this pattern.

### 10. **Command Pattern**
   - **Usage**: Encapsulates a request as an object, allowing for parameterization of clients with queues, requests, and operations.
   - **Spring Boot**: Often used in conjunction with messaging (RabbitMQ, Kafka) and task scheduling.
   - **Example**: In service-oriented architectures where actions or commands need to be queued or scheduled.

These design patterns work well with Spring Boot’s features and are common in modern application development, ensuring cleaner code, better separation of concerns, and easier maintainability.

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
