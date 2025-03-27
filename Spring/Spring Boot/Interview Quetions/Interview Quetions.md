**1. What is the difference between `@RestController` and `@Controller` in Spring Boot?**

* **`@Controller`**:
  * This annotation is used to indicate that a class is a Spring MVC controller.
  * It's primarily used for building web applications that return views (typically HTML pages).
  * Methods within a `@Controller` class often return a `ModelAndView` object or a String representing the view name.
  * You might use `@Controller` when you need to render server-side templates (like Thymeleaf, FreeMarker, or JSP) to create dynamic web pages.
* **`@RestController`**:
  * This annotation is a specialized version of `@Controller`.
  * It combines `@Controller` and `@ResponseBody`.
  * It's used for building RESTful web services that return data (typically JSON or XML) directly in the response body.
  * Methods within a `@RestController` class don't return views; they return data that's converted into the desired format.
  * This is the standard annotation for creating APIs.
  * In a nutshell `@RestController` is used to create APIs, and `@Controller` is used to create web pages.

**2. What is an IOC container?**

* **IOC (Inversion of Control) container** is a core concept in Spring framework.
* It's responsible for managing the lifecycle of Spring beans (objects created by the Spring framework).
* Instead of your application code creating and managing objects, the IOC container creates and injects them.
* This "inversion" means that the control of object creation is transferred from your application code to the container.
* The Spring IOC container uses dependency injection (DI) to provide beans with their dependencies.
* The most common implementations of the Spring IOC container are `BeanFactory` and `ApplicationContext` (which is a more advanced version of `BeanFactory`).
* In short, the IOC container manages the objects of your application.

**3. Describe the flow of HTTPS requests through the Spring Boot application?**

1. **Client Request:** A client (e.g., a web browser) sends an HTTPS request to the Spring Boot application.
2. **SSL/TLS Handshake:**
   * The request is received by the server (which might be an embedded server like Tomcat or Jetty, or an external server like Nginx or Apache).
   * The server and client perform an SSL/TLS handshake to establish a secure, encrypted connection.
   * This involves exchanging certificates and agreeing on encryption algorithms.
3. **Decryption:** The server decrypts the HTTPS request.
4. **Spring DispatcherServlet:** The decrypted request is forwarded to the Spring `DispatcherServlet`, which is the front controller in Spring MVC.
5. **Handler Mapping:** The `DispatcherServlet` uses handler mappings to determine which controller method should handle the request based on the URL and HTTP method.
6. **Controller Execution:** The appropriate controller method is executed.
7. **Service and Business Logic:** The controller may invoke service layer components to perform business logic.
8. **View Resolution (if applicable) or Data Serialization:**
   * If the controller returns a view name (for `@Controller`), the `DispatcherServlet` uses a view resolver to render the view.
   * If the controller returns data (for `@RestController`), the data is serialized into the appropriate format (e.g., JSON) using message converters.
9. **Response:** The `DispatcherServlet` sends the response back to the client.
10. **Encryption:** The server encrypts the response using SSL/TLS.
11. **Client Decryption:** The client receives the encrypted response and decrypts it.

**4. What is the difference between `RequestMapping` and `GetMapping`?**

* **`@RequestMapping`**:
  * This is a more general-purpose annotation used to map HTTP requests to controller methods.
  * It can handle any HTTP method (GET, POST, PUT, DELETE, etc.).
  * You can specify the HTTP method using the `method` attribute (e.g., `@RequestMapping(value = "/users", method = RequestMethod.GET)`).
* **`@GetMapping`**:
  * This is a specialized version of `@RequestMapping` that specifically handles HTTP GET requests.
  * It's a shorthand notation for `@RequestMapping(method = RequestMethod.GET)`.
  * It simplifies the code and makes it more readable.
  * Similarly Spring provides `@PostMapping`, `@PutMapping`, `@DeleteMapping` for their respective HTTP request types.
* In short, `@GetMapping` is a specialized form of `@RequestMapping` for GET requests.

**5. What is the use of Profiles in Spring Boot?**

* Spring Profiles allow you to configure different application behaviors based on the environment (e.g., development, testing, production).
* You can define different sets of configurations (beans, properties, etc.) and activate them based on the active profile.
* This is useful for:
  * Using different database configurations for development and production.
  * Enabling or disabling certain features based on the environment.
  * Loading different property files for different environments.
* Profiles are activated using the `spring.profiles.active` property (e.g., `spring.profiles.active=dev`).

**6. What is Spring Actuator? What are its advantages?**

* Spring Boot Actuator provides production-ready features to monitor and manage your Spring Boot application.
* It exposes endpoints that provide information about the application's health, metrics, environment, and more.
* **Advantages:**
  * **Monitoring:** Provides insights into the application's health and performance.
  * **Management:** Allows you to manage application settings and configurations.
  * **Troubleshooting:** Helps identify and diagnose issues.
  * **Security:** Provides secure access to sensitive information.

**7. How to enable Actuator in Spring Boot application?**

* To enable Spring Boot Actuator, add the `spring-boot-starter-actuator` dependency to your `pom.xml` (for Maven) or `build.gradle` (for Gradle).

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```gradle
implementation 'org.springframework.boot:spring-boot-starter-actuator'
```

* Once this dependancy is added, the actuator endpoints will be available. You can control the endpoints that are exposed through the management.endpoints.web.exposure.include property.

**8. What are the actuator-provided endpoints used for monitoring the Spring Boot application?**

* Some common actuator endpoints include:
  * `/actuator/health`: Shows the application's health status.
  * `/actuator/info`: Displays application information.
  * `/actuator/metrics`: Provides application metrics (e.g., memory usage, request counts).
  * `/actuator/beans`: Lists all the Spring beans in the application context.
  * `/actuator/env`: Shows the application's environment properties.
  * `/actuator/loggers`: Manages application loggers.
  * `/actuator/threaddump`: Provides a thread dump of the application.
  * `/actuator/heapdump`: provides a heap dump of the application.

**9. How to get the list of all the beans in your Spring Boot application?**

* You can use the `/actuator/beans` endpoint to get a list of all the Spring beans.
* Programmatically, you can inject `ApplicationContext` and use the `getBeanDefinitionNames()` method:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Arrays;

@RestController
public class BeanController {

    @Autowired
    private ApplicationContext applicationContext;

    @GetMapping("/beans")
    public String getBeans() {
        return Arrays.toString(applicationContext.getBeanDefinitionNames());
    }
}
```

**10. How to check the environment properties in your Spring Boot application?**

* You can use the `/actuator/env` endpoint to get the environment properties.
* Programmatically, you can inject `Environment` and access properties:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class EnvController {

    @Autowired
    private Environment environment;

    @GetMapping("/envProperty")
    public String getEnvProperty() {
        return environment.getProperty("spring.datasource.url");
    }
}
```

Absolutely! Let's delve into these Spring Boot concepts in detail:

**11. How to enable debugging log in the Spring Boot application?**

Spring Boot uses Logback by default. To enable debug logging, you can configure the logging level in your `application.properties` or `application.yml` file.

* **Using `application.properties`:**

  ```properties
  logging.level.root=DEBUG
  # Or for a specific package:
  logging.level.com.example=DEBUG
  ```
* **Using `application.yml`:**

  ```yaml
  logging:
    level:
      root: DEBUG
      com.example: DEBUG
  ```

  * `logging.level.root=DEBUG` sets the root logger to debug level, which will log debug messages from all packages.
  * `logging.level.com.example=DEBUG` sets the debug level for a specific package, such as `com.example`. This will only log debug messages from classes within that package.
  * You can also set the logging level for specific loggers.

**12. Where do we define properties in the Spring Boot application?**

Spring Boot provides several ways to define properties:

* **`application.properties` or `application.yml`:**
  * These are the most common ways to define properties.
  * `application.properties` uses key-value pairs, while `application.yml` uses a hierarchical structure.
  * These files are typically placed in the `src/main/resources` directory.
* **Command-line arguments:**
  * You can pass properties as command-line arguments when running the application.
  * For example: `java -jar myapp.jar --server.port=8081`.
* **Environment variables:**
  * Spring Boot can read properties from environment variables.
  * For example, setting the `SERVER_PORT` environment variable will override the `server.port` property.
* **Externalized Configuration:**
  * Spring boot can also read configuration from external files, and locations.
* **@Value Annotation:**
  * This annotation injects property values directly into fields.

**13. What is dependency Injection?**

* **Dependency Injection (DI)** is a design pattern that promotes loose coupling between software components.
* Instead of an object creating its dependencies, the dependencies are "injected" into the object.
* In Spring, the IOC container manages the creation and injection of dependencies.
* **Benefits:**
  * Reduced coupling: Objects are less dependent on each other.
  * Improved testability: Dependencies can be easily mocked or stubbed for unit testing.
  * Increased code reusability.
  * Simplified maintenance.
* In short, DI is a way of providing an object with its dependencies.

**14. Explain `@RestController` annotation in Spring Boot?**

* `@RestController` is a convenience annotation in Spring Boot that combines `@Controller` and `@ResponseBody`.
* It's used to create RESTful web services that return data (typically JSON or XML) directly in the HTTP response body.
* **Key features:**
  * Indicates that a class is a Spring MVC controller.
  * Automatically serializes the return value of methods into the desired format (e.g., JSON) using message converters.
  * Eliminates the need to use `@ResponseBody` on every method.
  * It is the standard annotation for building RESTful APIs.

**15. How to disable a specific auto-configuration class?**

* Spring Boot automatically configures many beans based on the dependencies on the classpath.
* To disable a specific auto-configuration class, use the `@EnableAutoConfiguration(exclude = {MyAutoConfiguration.class})` or `@SpringBootApplication(exclude = {MyAutoConfiguration.class})` annotation.

```java
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableAutoConfiguration(exclude = {org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration.class})
public class MyConfig {
    // ...
}
```

* This example disables the `DataSourceAutoConfiguration` class, which automatically configures a data source.

**16. Can we disable the default web server in the Spring Boot application?**

* Yes, you can disable the default embedded web server (Tomcat, Jetty, or Undertow) in Spring Boot.
* To do this, you need to change the application's web application type to `WebApplicationType.NONE`.
* You can do this programmatically or through the `application.properties` or `application.yml` file.
* **Programmatically:**

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.WebApplicationType;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class NonWebApplication {

    public static void main(String[] args) {
        SpringApplication application = new SpringApplication(NonWebApplication.class, args);
        application.setWebApplicationType(WebApplicationType.NONE);
        application.run(args);
    }
}
```

* **`application.properties`:**

```properties
spring.main.web-application-type=NONE
```

* **`application.yml`:**

```yaml
spring:
  main:
    web-application-type: NONE
```

**17. Can we override or replace the Embedded tomcat server in Spring Boot?**

* Yes, you can override or replace the embedded Tomcat server with other embedded servers like Jetty or Undertow.
* To do this, you need to exclude the `spring-boot-starter-tomcat` dependency and include the dependency for the desired server.
* **Example (replacing Tomcat with Jetty):**

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

**18. What is the default port of tomcat in spring boot?**

* The default port for the embedded Tomcat server in Spring Boot is **8080**.

**19. Is it possible to change the port of the embedded Tomcat server in Spring Boot?**

* Yes, you can change the port of the embedded Tomcat server using the `server.port` property.
* You can set this property in your `application.properties` or `application.yml` file.
* **`application.properties`:**

```properties
server.port=8081
```

* **`application.yml`:**

```yaml
server:
  port: 8081
```

**20. Can we create a non-web application in Spring Boot?**

* Yes, you can create a non-web application in Spring Boot.
* As mentioned in question 16 you can set the web application type to `WebApplicationType.NONE`, and therefore create applications that are command line tools, or background processing applications.

**21. What is Spring Boot dependency management?**

* Spring Boot's dependency management simplifies the process of managing dependencies in your project.
* It provides a curated set of dependencies that are tested and compatible with each other.
* **Key features:**
  * Automatic version management: Spring Boot manages the versions of dependencies, so you don't have to specify them explicitly.
  * Starter POMs: Spring Boot provides starter POMs (Project Object Models) that group related dependencies together.
  * Consistent dependency versions: Ensures that all dependencies in your project are compatible.
  * In short, Spring Boot dependency management makes it easier to manage the dependencies of your project.

**22. What Are the Basic Annotations that Spring Boot Offers?**

Spring Boot builds upon Spring Framework, and introduces many of its own annotations. Some of the basic and frequently used annotations include:

* **`@SpringBootApplication`**: Combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`. It is the main annotation of a Spring Boot application.
* **`@RestController`**: Indicates that a class is a RESTful controller.
* **`@Controller`**: Indicates that a class is a Spring MVC controller.
* **`@Service`**: Indicates that a class is a service component.
* **`@Repository`**: Indicates that a class is a data repository component.
* **`@Component`**: A generic stereotype for any Spring-managed component.
* **`@Autowired`**: Used for dependency injection.
* **`@Value`**: Used to inject property values.
* **`@RequestMapping`**, `@GetMapping`, `@PostMapping`,

**23. What is Spring Boot Starters and why are they important?**

* **Answer:**
  * Spring Boot Starters are dependency descriptors that simplify the process of adding dependencies to your project.
  * They bundle together common dependencies needed for specific functionalities (e.g., web development, data access, security).
  * **Importance:**
    * Reduces dependency management overhead.
    * Ensures compatibility between dependencies.
    * Saves development time by providing pre-configured dependencies.
    * For example the `spring-boot-starter-web` pulls in all the dependencies needed to create a web application.

**24. How does Spring Boot handle static resources (CSS, JavaScript, images)?**

* **Answer:**
  * Spring Boot automatically serves static resources from the following directories in the classpath:
    * `/static`
    * `/public`
    * `/resources`
    * `/META-INF/resources`
  * You can place your static files in any of these directories, and Spring Boot will handle serving them.
  * Spring boot also provides configuration options to customize the static resource handling.

**25. What is Spring Boot CLI?**

* **Answer:**
  * Spring Boot CLI (Command Line Interface) is a tool that allows you to quickly prototype Spring Boot applications from the command line.
  * It uses Groovy scripts to simplify development.
  * It provides features like dependency resolution, running applications, and creating projects.
  * It is very useful for rapid prototyping.

**26. How do you handle exceptions globally in Spring Boot?**

* **Answer:**
  * Spring Boot provides several ways to handle exceptions globally:
    * **`@ControllerAdvice`** : Allows you to create a global exception handler that applies to all controllers.
    * **`@ExceptionHandler`** : Used within a `@ControllerAdvice` class to handle specific exceptions.
    * **`@ResponseStatus`** : Allows you to set the HTTP status code for an exception.
    * **`SimpleMappingExceptionResolver`** : can be configured to map exception class names to view names.

**27. What is Spring Boot DevTools and what are its benefits?**

* **Answer:**
  * Spring Boot DevTools is a set of tools that improves the development experience.
  * **Benefits:**
    * Automatic application restarts when code changes are detected.
    * LiveReload functionality for browser refreshes.
    * Default property configuration for development.
    * It helps to speed up the development cycle.

**28. How do you secure a Spring Boot application?**

* **Answer:**
  * Spring Boot provides integration with Spring Security to secure applications.
  * You can use Spring Security to implement authentication and authorization.
  * Spring Security supports various authentication mechanisms, including basic authentication, form-based authentication, OAuth2, and JWT.
  * Spring security is very flexible, and very powerful.

**29. How does Spring Boot handle file uploads?**

* **Answer:**
  * Spring Boot provides built-in support for file uploads using `MultipartFile`.
  * You can handle file uploads in your controllers by accepting `MultipartFile` parameters.
  * Spring boot simplifies the process of receiving and storing uploaded files.

**30. What are Spring Boot Actuator custom endpoints?**

* **Answer:**
  * While Spring Boot Actuator provides many built in endpoints, you can create custom endpoints to expose application specific information.
  * This is done by creating a class that is annotated with `@Endpoint`, `@WebEndpoint`, or `@RestControllerEndpoint`, and then adding methods annotated with `@ReadOperation`, `@WriteOperation`, or `@DeleteOperation` to the class.
  * This allows you to add monitoring or management capabilities that are specific to your application.

**31. How to implement caching in Spring Boot?**

* **Answer:**
  * Spring Boot provides an abstraction layer for caching.
  * You can use annotations like `@EnableCaching`, `@Cacheable`, `@CachePut`, and `@CacheEvict` to enable and configure caching.
  * Spring Boot supports various caching providers, including Caffeine, EhCache, Redis, and Hazelcast.
  * Caching can improve the performance of your application.

**32. What is Spring Boot Test?**

* **Answer:**
  * Spring Boot Test is a module that provides support for testing Spring Boot applications.
  * It includes features like:
    * `@SpringBootTest`: Loads the full application context for integration tests.
    * `@WebMvcTest`: Tests Spring MVC controllers without loading the full context.
    * `@DataJpaTest`: Tests JPA repositories.
    * `MockMvc`: Allows you to test web requests.
  * Spring boot test makes it very easy to create tests for your application.


**33. How do you implement message queues (e.g., RabbitMQ, Kafka) in Spring Boot?**

* **Answer:**
  * Spring Boot simplifies integration with message queues using Spring AMQP (for RabbitMQ) and Spring for Apache Kafka.
  * You add the corresponding starter dependency (`spring-boot-starter-amqp` or `spring-kafka`).
  * You can use annotations like `@RabbitListener` or `@KafkaListener` to define message consumers.
  * `RabbitTemplate` or `KafkaTemplate` can be used to send messages.
  * Spring Boot handles the configuration and connection management, making it easy to produce and consume messages.

**34. What is Spring Boot Actuator's Health Indicator?**

* **Answer:**
  * Health Indicators are components that provide information about the health of different parts of your application.
  * Actuator's `/actuator/health` endpoint aggregates the results of all registered Health Indicators.
  * Spring Boot provides default Health Indicators for components like data sources, message queues, and disk space.
  * You can create custom Health Indicators to monitor application-specific components.
  * This is very valuable for monitoring the health of your application in a production environment.

**35. How do you implement distributed tracing in Spring Boot (e.g., using Spring Cloud Sleuth and Zipkin/Jaeger)?**

* **Answer:**
  * Distributed tracing helps you track requests as they flow through a distributed system.
  * Spring Cloud Sleuth adds trace and span IDs to your logs and HTTP requests.
  * Zipkin or Jaeger are used to collect and visualize the trace data.
  * You add the necessary starter dependencies (`spring-cloud-starter-sleuth`, `spring-cloud-sleuth-zipkin`, or `spring-cloud-sleuth-jaeger`).
  * Spring Boot automatically configures the tracing infrastructure.
  * This is very helpful to debug issues in microservice architectures.

**36. Explain the concept of Spring Boot Auto-configuration. How does it work?**

* **Answer:**
  * Auto-configuration is a core feature of Spring Boot that automatically configures your application based on the dependencies on the classpath.
  * Spring Boot looks for classes with the `@Configuration` annotation and specific conditions (e.g., presence of certain classes or properties).
  * If the conditions are met, the auto-configuration class creates and registers beans in the application context.
  * This reduces the amount of manual configuration required.
  * For Example if a database driver is found in the classpath, Spring boot will attempt to configure a datasource.

**37. How do you handle asynchronous tasks in Spring Boot?**

* **Answer:**
  * Spring Boot provides support for asynchronous task execution using the `@Async` annotation.
  * You enable asynchronous processing by adding the `@EnableAsync` annotation to a configuration class.
  * Methods annotated with `@Async` are executed in a separate thread.
  * You can configure the thread pool used for asynchronous tasks.
  * This is very helpful for long running tasks, that should not block the main thread of the application.

**38. What are Spring Boot's externalized configurations?**

* **Answer:**
  * Spring Boot supports externalized configurations, which allow you to configure your application from various sources outside the application's code.
  * Supported sources include:
    * `application.properties` and `application.yml` files.
    * Command-line arguments.
    * Environment variables.
    * External configuration files.
    * JNDI attributes.
  * This allows you to easily change configuration settings without rebuilding or redeploying your application.

**39. How do you implement internationalization (i18n) in Spring Boot?**

* **Answer:**
  * Spring Boot provides support for internationalization using `MessageSource`.
  * You create message property files for different languages (e.g., `messages.properties`, `messages_fr.properties`).
  * You configure a `MessageSource` bean to load the message files.
  * You can use `LocaleContextHolder` to determine the user's locale.
  * This allows your application to support multiple languages.

**40. What are Spring Boot's embedded databases, and when would you use them?**

* **Answer:**
  * Spring Boot provides support for embedded databases like H2, HSQLDB, and Derby.
  * Embedded databases are in-memory databases that are created and destroyed when the application starts and stops.
  * They are useful for:
    * Development and testing.
    * Rapid prototyping.
    * Applications with simple data storage requirements.
  * They simplify the setup and configuration of databases during development.


**41. What is the primary purpose of `bootstrap.yml` (or `bootstrap.properties`) in a Spring Boot application?**

* **Answer:** The primary purpose of `bootstrap.yml` (or `bootstrap.properties`) is to provide configuration properties that are loaded very early in the Spring Boot application's startup process, *before* the `application.yml` (or `application.properties`) file. This is particularly crucial for configurations needed to initialize the application context itself, such as connecting to a Spring Cloud Config server.

**42. When would you typically use `bootstrap.yml` instead of `application.yml`?**

* **Answer:**
  * When you need to configure properties that are required before the main application context is initialized.
  * Specifically, when using Spring Cloud Config to fetch externalized configuration, the connection details for the config server should reside in `bootstrap.yml`.
  * For any early stage encryption or decryption setup.
  * Essentially, for any configuration that must be available before the `application.yml` file is processed.

**43. How does the order of loading configuration files (e.g., `bootstrap.yml`, `application.yml`) affect property values?**

* **Answer:**
  * Spring Boot loads `bootstrap.yml` (or `bootstrap.properties`) first.
  * Then, it loads `application.yml` (or `application.properties`).
  * Properties defined in `application.yml` will override any properties with the same name that are defined in `bootstrap.yml`.
  * This loading order is important to remember when troubleshooting configuration issues.

**44. Can you provide an example of a common configuration in `bootstrap.yml` related to Spring Cloud Config?**

* **Answer:**

  **YAML**

  ```
  spring:
    cloud:
      config:
        uri: http://config-server:8888
        name: my-application
        profile: development
  ```

  * This configuration tells the Spring Boot application where to find the Spring Cloud Config server (`uri`), the name of the application (`name`), and the active profile (`profile`).

**45. What happens if a property is defined in both `bootstrap.yml` and `application.yml` with different values?**

* **Answer:** The value from `application.yml` will be used. Spring Boot gives precedence to properties defined in `application.yml` over those in `bootstrap.yml`.

**46. Is `bootstrap.yml` mandatory in a Spring Boot application?**

* **Answer:** No, `bootstrap.yml` (or `bootstrap.properties`) is optional. If your application doesn't require any early-stage configuration, especially related to Spring Cloud Config or similar setups, you don't need to create it.

**47. How do environment-specific `bootstrap.yml` files work (e.g., `bootstrap-dev.yml`, `bootstrap-prod.yml`)?**

* **Answer:**
  * Similar to `application.yml`, you can create environment-specific `bootstrap.yml` files.
  * Spring Boot will load the appropriate file based on the active Spring profile.
  * For example, if `spring.profiles.active=dev`, Spring Boot will load `bootstrap-dev.yml`.
  * This allows you to have different early stage configurations for different environments.

**48. Can `bootstrap.yml` be used for configurations other than Spring Cloud Config?**

* **Answer:** Yes. While Spring Cloud Config is the most common use case, `bootstrap.yml` can be used for any configuration that needs to be loaded early in the application's lifecycle. This includes configurations related to security, custom property sources, or any other early initialization logic.

**49. What are the differences between `.yml` and `.properties` for bootstrap configuration?**

* **Answer:**
  * `.yml` uses YAML syntax, which is hierarchical and more readable for complex configurations.
  * `.properties` uses a flat key-value pair format.
  * Both serve the same purpose for spring boot, and the use of one over the other is mostly a matter of preference.

**50. What is the impact of a missing or misconfigured `bootstrap.yml` when using Spring Cloud Config?**

* **Answer:**
  * If `bootstrap.yml` is missing or misconfigured when using Spring Cloud Config, the application will likely fail to start.
  * It won't be able to connect to the config server, and therefore, it won't be able to retrieve its externalized configuration.
  * This can result in exceptions or errors during application startup.
