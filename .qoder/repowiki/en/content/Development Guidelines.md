# Development Guidelines

<cite>
**Referenced Files in This Document**   
- [nohttp-checkstyle.xml](file://src/checkstyle/nohttp-checkstyle.xml)
- [nohttp-checkstyle-suppressions.xml](file://src/checkstyle/nohttp-checkstyle-suppressions.xml)
- [pom.xml](file://pom.xml)
- [build.gradle](file://build.gradle)
- [README.md](file://README.md)
- [PetClinicApplication.java](file://src/main/java/org/springframework/samples/petclinic/PetClinicApplication.java)
- [OwnerController.java](file://src/main/java/org/springframework/samples/petclinic/owner/OwnerController.java)
- [OwnerControllerTests.java](file://src/test/java/org/springframework/samples/petclinic/owner/OwnerControllerTests.java)
- [CrashController.java](file://src/main/java/org/springframework/samples/petclinic/system/CrashController.java)
- [CrashControllerTests.java](file://src/test/java/org/springframework/samples/petclinic/system/CrashControllerTests.java)
- [CrashControllerIntegrationTests.java](file://src/test/java/org/springframework/samples/petclinic/system/CrashControllerIntegrationTests.java)
- [MySqlIntegrationTests.java](file://src/test/java/org/springframework/samples/petclinic/MySqlIntegrationTests.java)
- [PostgresIntegrationTests.java](file://src/test/java/org/springframework/samples/petclinic/PostgresIntegrationTests.java)
- [PetClinicIntegrationTests.java](file://src/test/java/org/springframework/samples/petclinic/PetClinicIntegrationTests.java)
- [application.properties](file://src/main/resources/application.properties)
- [.editorconfig](file://.editorconfig)
</cite>

## Table of Contents
1. [Code Style and Checkstyle Configuration](#code-style-and-checkstyle-configuration)
2. [Contribution Process](#contribution-process)
3. [Development Environment Setup](#development-environment-setup)
4. [Running Tests](#running-tests)
5. [Debugging Common Development Issues](#debugging-common-development-issues)
6. [HTTP vs HTTPS and Cross-Origin Considerations](#http-vs-https-and-cross-origin-considerations)
7. [Writing Effective Tests](#writing-effective-tests)
8. [Performance Optimization for Development](#performance-optimization-for-development)
9. [Developer Certificate of Origin (DCO)](#developer-certificate-of-origin-dco)

## Code Style and Checkstyle Configuration

The PetClinic application enforces strict code style rules through the Checkstyle configuration defined in `nohttp-checkstyle.xml`. This configuration is integrated into both Maven and Gradle build systems to ensure consistent code quality across contributions.

The primary purpose of the `nohttp-checkstyle.xml` configuration is to prevent the use of HTTP URLs in the codebase, enforcing HTTPS for all external resource references. This is implemented through the `NoHttpCheck` module from the `io.spring.nohttp.checkstyle.check` package. The configuration applies to all source files in the project, with exclusions for binary files and build directories as specified in the `maven-checkstyle-plugin` configuration in `pom.xml`.

The Checkstyle rules are enforced during the Maven `validate` phase and are integrated into the Gradle build via the `checkstyle` and `checkstyleNohttp` tasks. Developers should ensure their IDEs are configured to use the same Checkstyle configuration to catch violations early. The `.editorconfig` file provides additional editor-specific formatting preferences including indentation style, line endings, and charset encoding.

**Section sources**
- [nohttp-checkstyle.xml](file://src/checkstyle/nohttp-checkstyle.xml)
- [pom.xml](file://pom.xml#L200-L235)
- [build.gradle](file://build.gradle#L62-L89)
- [.editorconfig](file://.editorconfig)

## Contribution Process

Contributions to the PetClinic application are managed through GitHub pull requests. The preferred channel for submitting contributions is the [issue tracker](https://github.com/spring-projects/spring-petclinic/issues), where bug reports, feature requests, and pull requests are reviewed.

Before submitting a contribution, developers should:
1. Fork the repository and create a feature branch
2. Make changes following the code style guidelines
3. Run all tests to ensure existing functionality is not broken
4. Commit changes with descriptive messages
5. Include the DCO sign-off in each commit
6. Submit a pull request to the main repository

The build process automatically validates code style using Checkstyle, and pull requests will fail if violations are detected. Contributors should address all Checkstyle warnings before submitting their pull request.

**Section sources**
- [README.md](file://README.md#L144-L163)
- [pom.xml](file://pom.xml)
- [build.gradle](file://build.gradle)

## Development Environment Setup

To set up a development environment for the PetClinic application, the following prerequisites must be installed:
- Java 17 or newer (full JDK, not a JRE)
- Git command line tool
- An IDE such as IntelliJ IDEA, Eclipse with m2e plugin, Spring Tools Suite, or VS Code

The application can be imported into an IDE using the build configuration files. For Maven users, import the project as an "Existing Maven Project" using the `pom.xml` file. For Gradle users, open the `build.gradle` file directly in the IDE.

Once imported, the application can be run from the IDE by executing the `main` method in `PetClinicApplication.java`. The application will start on port 8080 and be accessible at `http://localhost:8080`.

**Section sources**
- [README.md](file://README.md#L80-L101)
- [PetClinicApplication.java](file://src/main/java/org/springframework/samples/petclinic/PetClinicApplication.java)

## Running Tests

The PetClinic application includes a comprehensive test suite that can be executed using either Maven or Gradle. Unit tests and integration tests are located in the `src/test/java` directory and are organized by package structure.

To run tests with Maven:
```bash
./mvnw test
```

To run tests with Gradle:
```bash
./gradlew test
```

The test suite includes profile-specific integration tests for different database configurations:
- `PetClinicIntegrationTests`: Uses the default H2 in-memory database
- `MySqlIntegrationTests`: Uses MySQL via Testcontainers
- `PostgresIntegrationTests`: Uses PostgreSQL via Docker Compose

These integration tests can be run directly from the IDE as Java applications, providing fast feedback during development. The `MySqlIntegrationTests` class uses Testcontainers to start a MySQL database in a Docker container, while `PostgresIntegrationTests` uses Spring Boot's Docker Compose support.

**Section sources**
- [MySqlIntegrationTests.java](file://src/test/java/org/springframework/samples/petclinic/MySqlIntegrationTests.java)
- [PostgresIntegrationTests.java](file://src/test/java/org/springframework/samples/petclinic/PostgresIntegrationTests.java)
- [PetClinicIntegrationTests.java](file://src/test/java/org/springframework/samples/petclinic/PetClinicIntegrationTests.java)

## Debugging Common Development Issues

### Controller Mapping Issues
When experiencing controller mapping issues, verify that the controller class is properly annotated with `@Controller` and that request mapping annotations (`@GetMapping`, `@PostMapping`, etc.) are correctly specified. Ensure that the controller is in the component scan path of the Spring application. The `OwnerController` class provides an example of proper controller implementation with request mappings.

### Database Connectivity Problems
Database connectivity issues can occur when switching between different database profiles. Ensure the correct profile is active (`spring.profiles.active=h2`, `mysql`, or `postgres`) and that the corresponding database service is running. For MySQL and PostgreSQL, use the provided `docker-compose.yml` file to start the database containers:
```bash
docker compose up mysql
# or
docker compose up postgres
```

### Template Rendering Errors
Template rendering errors typically occur due to incorrect Thymeleaf template paths or missing model attributes. Verify that templates are located in `src/main/resources/templates` and that model attributes are properly added to the `ModelAndView` object in controller methods. The `OwnerController.showOwner` method demonstrates proper template rendering with model attribute binding.

**Section sources**
- [OwnerController.java](file://src/main/java/org/springframework/samples/petclinic/owner/OwnerController.java)
- [CrashController.java](file://src/main/java/org/springframework/samples/petclinic/system/CrashController.java)
- [application.properties](file://src/main/resources/application.properties)

## HTTP vs HTTPS and Cross-Origin Considerations

The Checkstyle configuration enforces the use of HTTPS over HTTP for all external resource references through the `NoHttpCheck` rule. This prevents security vulnerabilities associated with unencrypted connections. The suppression configuration in `nohttp-checkstyle-suppressions.xml` excludes binary files and build directories from this check.

For cross-origin requests, the application relies on Spring Boot's default security configuration. When developing APIs that need to support cross-origin requests, consider implementing CORS configuration through Spring's `@CrossOrigin` annotation or by configuring a `WebMvcConfigurer` bean. The current PetClinic implementation does not include explicit CORS configuration as it is primarily a server-rendered application.

**Section sources**
- [nohttp-checkstyle.xml](file://src/checkstyle/nohttp-checkstyle.xml)
- [nohttp-checkstyle-suppressions.xml](file://src/checkstyle/nohttp-checkstyle-suppressions.xml)

## Writing Effective Tests

The PetClinic application follows best practices for writing effective unit and integration tests. Unit tests focus on isolated business logic and use mocking to isolate dependencies, as demonstrated in `OwnerControllerTests` which uses Mockito to mock the `OwnerRepository`.

Integration tests verify the complete application flow with real components. The `CrashControllerIntegrationTests` class demonstrates how to test controller behavior with different content types (JSON and HTML) using `TestRestTemplate`. Key practices include:
- Using descriptive test method names
- Testing both success and failure scenarios
- Validating HTTP status codes and response content
- Using appropriate test slices (`@WebMvcTest`, `@DataJpaTest`, etc.)

When writing new tests, follow the existing patterns in the codebase and ensure adequate test coverage for both positive and negative scenarios.

**Section sources**
- [OwnerControllerTests.java](file://src/test/java/org/springframework/samples/petclinic/owner/OwnerControllerTests.java)
- [CrashControllerTests.java](file://src/test/java/org/springframework/samples/petclinic/system/CrashControllerTests.java)
- [CrashControllerIntegrationTests.java](file://src/test/java/org/springframework/samples/petclinic/system/CrashControllerIntegrationTests.java)

## Performance Optimization for Development

To optimize the development workflow, consider the following performance suggestions:

### Incremental Compilation
Use the Spring Boot Maven or Gradle plugin to enable incremental compilation and fast application restarts:
```bash
./mvnw spring-boot:run
# or
./gradlew bootRun
```

This allows code changes to be picked up immediately without a full rebuild.

### Hot Reloading
Include Spring Boot Devtools as a test dependency to enable automatic application restarts when classpath files change. Devtools is already included in the `pom.xml` and `build.gradle` files.

### Selective Testing
During development, run specific test classes or methods rather than the entire test suite to get faster feedback. Most IDEs allow running individual test methods.

### CSS Compilation
When making changes to SCSS files, recompile CSS using the Maven CSS profile:
```bash
./mvnw package -P css
```
Note that there is no equivalent Gradle profile for CSS compilation.

**Section sources**
- [pom.xml](file://pom.xml)
- [build.gradle](file://build.gradle)
- [README.md](file://README.md#L101-L108)

## Developer Certificate of Origin (DCO)

All contributions to the PetClinic application require a Developer Certificate of Origin (DCO) sign-off. This is implemented by adding a `Signed-off-by` trailer to each commit message, indicating that the contributor agrees to the DCO.

To sign off on a commit, use the `-s` flag with git commit:
```bash
git commit -s -m "Your commit message"
```

The DCO process replaces the traditional Contributor License Agreement (CLA) and simplifies the contribution process. By signing off on a commit, the contributor certifies that:
- They have the right to submit the work under the project's license
- The contribution is original work or properly licensed
- They understand the project may redistribute the contribution

For additional details, refer to the blog post [Hello DCO, Goodbye CLA: Simplifying Contributions to Spring](https://spring.io/blog/2025/01/06/hello-dco-goodbye-cla-simplifying-contributions-to-spring).

**Section sources**
- [README.md](file://README.md#L157-L163)
- [LICENSE.txt](file://LICENSE.txt)