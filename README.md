
---

# Spring Data JPA

[Spring Data JPA](https://projects.spring.io/spring-data), part of the larger [Spring Data](https://projects.spring.io/spring-data) family, makes it easy to implement JPA-based repositories. This module provides enhanced support for JPA-based data access layers, making it simpler to build Spring-powered applications that use data access technologies.

---

## Overview

Implementing a data access layer for an application can be cumbersome due to boilerplate code for executing queries, pagination, and auditing. Spring Data JPA minimizes this effort, allowing developers to focus on defining repository interfaces with custom finder methods. Spring provides the implementations automatically.

---

## Features

- Implementation of CRUD methods for JPA entities
- Dynamic query generation from query method names
- Transparent triggering of JPA NamedQueries by query methods
- Domain base classes providing basic properties
- Transparent auditing (e.g., creation and last modification timestamps)
- Custom repository code integration
- Seamless Spring integration with a custom namespace

---

## Code of Conduct

This project adheres to the [Spring Code of Conduct](https://github.com/spring-projects/.github/blob/e3cc2ff230d8f1dca06535aa6b5a4a23815861d4/CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Report unacceptable behavior to [spring-code-of-conduct@pivotal.io](mailto:spring-code-of-conduct@pivotal.io).

---

## Getting Started

Here's a quick example using Spring Data JPA repositories:

### Example Code

```java
public interface PersonRepository extends CrudRepository<Person, Long> {
    List<Person> findByLastname(String lastname);
    List<Person> findByFirstnameLike(String firstname);
}

@Service
public class MyService {

    private final PersonRepository repository;

    public MyService(PersonRepository repository) {
        this.repository = repository;
    }

    public void doWork() {
        repository.deleteAll();

        Person person = new Person();
        person.setFirstname("Oliver");
        person.setLastname("Gierke");
        repository.save(person);

        List<Person> lastNameResults = repository.findByLastname("Gierke");
        List<Person> firstNameResults = repository.findByFirstnameLike("Oli%");
    }
}

@Configuration
@EnableJpaRepositories("com.acme.repositories")
class AppConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder().setType(EmbeddedDatabaseType.H2).build();
    }

    @Bean
    public JpaTransactionManager transactionManager(EntityManagerFactory emf) {
        return new JpaTransactionManager(emf);
    }

    @Bean
    public JpaVendorAdapter jpaVendorAdapter() {
        HibernateJpaVendorAdapter jpaVendorAdapter = new HibernateJpaVendorAdapter();
        jpaVendorAdapter.setDatabase(Database.H2);
        jpaVendorAdapter.setGenerateDdl(true);
        return jpaVendorAdapter;
    }

    @Bean
    public LocalContainerEntityManagerFactoryBean entityManagerFactory() {
        LocalContainerEntityManagerFactoryBean lemfb = new LocalContainerEntityManagerFactoryBean();
        lemfb.setDataSource(dataSource());
        lemfb.setJpaVendorAdapter(jpaVendorAdapter());
        lemfb.setPackagesToScan("com.acme");
        return lemfb;
    }
}
```

### Maven Configuration

Add the following dependency to your `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
    <version>${version}</version>
</dependency>
```

For the latest snapshots:

```xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
    <version>${version}-SNAPSHOT</version>
</dependency>

<repository>
    <id>spring-snapshot</id>
    <name>Spring Snapshot Repository</name>
    <url>https://repo.spring.io/snapshot</url>
</repository>
```

---

## Getting Help

If you encounter issues or need guidance, check out the following resources:

- [Reference Documentation](https://docs.spring.io/spring-data/jpa/reference/)
- [API Documentation](https://docs.spring.io/spring-data/jpa/docs/current/api/)
- [Spring Guides](https://spring.io/guides)
- [StackOverflow](https://stackoverflow.com/questions/tagged/spring-data-jpa)
- [Gitter Community Chat](https://gitter.im/spring-projects/spring-data)
- [GitHub Issue Tracker](https://github.com/spring-projects/spring-data-jpa/issues)

---

## Reporting Issues

1. Search the [issue tracker](https://github.com/spring-projects/spring-data-jpa/issues) for existing reports.
2. If none exist, [create a new issue](https://github.com/spring-projects/spring-data-jpa/issues) with detailed information:
   - Spring Data JPA version
   - JVM version
   - Stack traces
   - Relevant configuration
   - Minimal reproducible example

---

## Building from Source

While pre-built binaries are available on [repo.spring.io](https://repo.spring.io), you can build from source if desired.

```bash
$ ./mvnw clean install
```

Requirements:
- JDK 17 or above
- [Maven 3.8.0+](https://maven.apache.org/)

To build reference documentation:

```bash
$ ./mvnw clean install -Pantora
```

The documentation will be available at `target/antora/site/index.html`.

---

## Guides

Explore Spring Data guides:

- [Accessing Data with JPA](https://spring.io/guides/gs/accessing-data-jpa/)
- [Accessing JPA Data with REST](https://spring.io/guides/gs/accessing-data-rest/)

---

## Examples

Visit the [Spring Data Examples repository](https://github.com/spring-projects/spring-data-examples) for detailed examples of various features.

---

## License

Spring Data JPA is open-source software released under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0.html).

--- 

This format ensures clarity and ease of navigation for users.
