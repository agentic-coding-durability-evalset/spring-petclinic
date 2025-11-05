# Getting Started

<cite>
**Referenced Files in This Document**   
- [README.md](file://README.md)
- [pom.xml](file://pom.xml)
- [build.gradle](file://build.gradle)
- [application.properties](file://src/main/resources/application.properties)
- [application-mysql.properties](file://src/main/resources/application-mysql.properties)
- [application-postgres.properties](file://src/main/resources/application-postgres.properties)
- [petclinic.scss](file://src/main/scss/petclinic.scss)
- [PetClinicApplication.java](file://src/main/java/org/springframework/samples/petclinic/PetClinicApplication.java)
- [docker-compose.yml](file://docker-compose.yml)
- [petclinic_db_setup_mysql.txt](file://src/main/resources/db/mysql/petclinic_db_setup_mysql.txt)
- [petclinic_db_setup_postgres.txt](file://src/main/resources/db/postgres/petclinic_db_setup_postgres.txt)
- [user.sql](file://src/main/resources/db/mysql/user.sql)
</cite>

## Table of Contents
1. [Prerequisites Installation](#prerequisites-installation)
2. [Cloning and Project Setup](#cloning-and-project-setup)
3. [Building and Running with Maven](#building-and-running-with-maven)
4. [Building and Running with Gradle](#building-and-running-with-gradle)
5. [Accessing the Application](#accessing-the-application)
6. [Database Configuration and Profiles](#database-configuration-and-profiles)
7. [Compiling SCSS to CSS](#compiling-scss-to-css)
8. [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
9. [Performance Tips with Spring Boot DevTools](#performance-tips-with-spring-boot-devtools)

## Prerequisites Installation

Before running the Spring PetClinic application, ensure the following prerequisites are installed on your system:

- **Java 17 or newer**: The application requires at least Java 17. The `pom.xml` and `build.gradle` files specify Java 17 as the source compatibility level. Using a JRE instead of a full JDK may cause build issues.
- **Git**: Required to clone the repository from GitHub. Install the Git command-line tool from [git-scm.com](https://git-scm.com).
- **IDE**: Choose one of the following supported IDEs:
  - Eclipse with m2e plugin
  - Spring Tools Suite (STS)
  - IntelliJ IDEA
  - VS Code

Verify Java installation by running `java -version` and `javac -version` in your terminal. Ensure both report version 17 or higher.

**Section sources**
- [README.md](file://README.md#L100-L117)
- [pom.xml](file://pom.xml#L15-L17)
- [build.gradle](file://build.gradle#L14-L16)

## Cloning and Project Setup

To begin, clone the Spring PetClinic repository from GitHub:

```bash
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
```

After cloning, import the project into your preferred IDE:

- **IntelliJ IDEA**: Use `File -> Open` and select the `pom.xml` file for Maven projects or `build.gradle` for Gradle.
- **Eclipse/STS**: Use `File -> Import -> Maven -> Existing Maven Project` and select the project root directory.
- **VS Code**: Open the folder and ensure the Java and Spring extensions are installed.

The project structure includes both Maven (`pom.xml`) and Gradle (`build.gradle`) configuration files, allowing flexibility in build tool selection.

**Section sources**
- [README.md](file://README.md#L50-L53)
- [README.md](file://README.md#L119-L132)

## Building and Running with Maven

The Spring PetClinic application can be built and run using Apache Maven. The project includes a Maven wrapper (`mvnw`), eliminating the need for a local Maven installation.

To compile and run the application using Maven:

```bash
./mvnw spring-boot:run
```

This command starts the Spring Boot application directly, enabling live reload for faster development cycles. Any changes to source files will be automatically detected upon recompilation.

Alternatively, package the application into a JAR file and run it:

```bash
./mvnw package
java -jar target/*.jar
```

The `pom.xml` file defines the Spring Boot Maven plugin, which handles packaging and execution.

**Section sources**
- [README.md](file://README.md#L55-L65)
- [pom.xml](file://pom.xml)

## Building and Running with Gradle

For Gradle users, the application can be built and executed using the Gradle wrapper (`gradlew`).

To build and run the application:

```bash
./gradlew bootRun
```

This command uses the Spring Boot Gradle plugin to start the application. Like Maven, Gradle supports incremental builds and live reload when used with an IDE.

To package the application:

```bash
./gradlew build
```

The resulting JAR file will be located in the `build/libs/` directory.

**Section sources**
- [README.md](file://README.md#L67-L69)
- [build.gradle](file://build.gradle)

## Accessing the Application

Once the application is running, it will be available at:

```
http://localhost:8080
```

Navigate to this URL in your web browser to access the PetClinic interface. The default in-memory H2 database is automatically populated with sample data on startup.

The H2 console is also available for database inspection at:

```
http://localhost:8080/h2-console
```

When accessing the H2 console, use the JDBC URL printed in the console output during startup, typically in the format `jdbc:h2:mem:<uuid>`. Leave the username as `sa` and password empty unless configured otherwise.

**Section sources**
- [README.md](file://README.md#L60-L61)
- [README.md](file://README.md#L80-L83)

## Database Configuration and Profiles

By default, the application uses an in-memory H2 database. To switch to a persistent database, activate the appropriate Spring profile:

- **MySQL**: Use `spring.profiles.active=mysql`
- **PostgreSQL**: Use `spring.profiles.active=postgres`

Set the active profile via command line:

```bash
./mvnw spring-boot:run -Dspring-boot.run.profiles=mysql
```

or

```bash
./mvnw spring-boot:run -Dspring-boot.run.profiles=postgres
```

Configuration files `application-mysql.properties` and `application-postgres.properties` contain the respective datasource settings. Environment variables such as `MYSQL_URL`, `MYSQL_USER`, and `MYSQL_PASS` can override defaults.

To start MySQL or PostgreSQL using Docker:

```bash
docker run -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic -e MYSQL_DATABASE=petclinic -p 3306:3306 mysql:9.1
```

or

```bash
docker run -e POSTGRES_USER=petclinic -e POSTGRES_PASSWORD=petclinic -e POSTGRES_DB=petclinic -p 5432:5432 postgres:17.0
```

Alternatively, use the provided `docker-compose.yml`:

```bash
docker compose up mysql
```

or

```bash
docker compose up postgres
```

**Section sources**
- [README.md](file://README.md#L77-L100)
- [application-mysql.properties](file://src/main/resources/application-mysql.properties)
- [application-postgres.properties](file://src/main/resources/application-postgres.properties)
- [docker-compose.yml](file://docker-compose.yml)
- [petclinic_db_setup_mysql.txt](file://src/main/resources/db/mysql/petclinic_db_setup_mysql.txt)
- [petclinic_db_setup_postgres.txt](file://src/main/resources/db/postgres/petclinic_db_setup_postgres.txt)
- [user.sql](file://src/main/resources/db/mysql/user.sql)

## Compiling SCSS to CSS

The application's CSS is generated from SCSS source files located in `src/main/scss/`. The main file is `petclinic.scss`, which imports other SCSS components and Bootstrap.

To recompile CSS after modifying SCSS files, use the Maven `css` profile:

```bash
./mvnw package -P css
```

This triggers the `libsass-maven-plugin` defined in `pom.xml`, which compiles SCSS to CSS and outputs to `src/main/resources/static/resources/css/petclinic.css`.

Note: There is no equivalent Gradle task for compiling SCSS. This functionality is only available via Maven.

**Section sources**
- [README.md](file://README.md#L146-L150)
- [pom.xml](file://pom.xml#L400-L436)
- [petclinic.scss](file://src/main/scss/petclinic.scss)

## Common Issues and Troubleshooting

### Missing Java Version
Ensure Java 17 or newer is installed. The `pom.xml` enforces this via the `maven-enforcer-plugin`. If an older version is detected, the build will fail with a clear error message.

### Port Conflicts
If port 8080 is already in use, the application will fail to start. Stop the conflicting process or change the server port by adding:

```bash
--server.port=8081
```

to the run command.

### Database Connection Errors
- For **MySQL**, ensure the `petclinic` database exists and the user has privileges. Run `user.sql` if needed.
- For **PostgreSQL**, verify the `petclinic` user and database are created.
- Check environment variables match those in `application-*.properties`.

### Build Failures
- Ensure Git is installed, as some build steps may depend on Git metadata.
- Clear local repository cache if dependency resolution fails: `./mvnw dependency:purge-local-repository`.

### Classpath Issues
If classes are not found, ensure dependencies are correctly resolved:
- For Maven: Run `./mvnw clean compile`
- For Gradle: Run `./gradlew clean compileJava`

**Section sources**
- [README.md](file://README.md#L77-L100)
- [pom.xml](file://pom.xml#L150-L165)
- [application-mysql.properties](file://src/main/resources/application-mysql.properties)
- [application-postgres.properties](file://src/main/resources/application-postgres.properties)

## Performance Tips with Spring Boot DevTools

For faster development cycles, the application includes `spring-boot-devtools` as a test-scoped dependency. When running in development mode (e.g., via `spring-boot:run`), DevTools provides:

- **Automatic restart**: Changes to classpath resources trigger a restart.
- **LiveReload**: Browser refreshes automatically when frontend resources change.
- **Disabled template caching**: Thymeleaf templates are reloaded without restart.

To maximize performance:
- Avoid packaging DevTools in production builds (it's already scoped to `test`).
- Use IDE hot-reload features in conjunction with DevTools.
- Disable DevTools in production by excluding the dependency or setting `spring.devtools.restart.enabled=false`.

**Section sources**
- [README.md](file://README.md#L141-L144)
- [pom.xml](file://pom.xml#L65-L67)
- [build.gradle](file://build.gradle#L25-L26)