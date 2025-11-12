# Spring PetClinic 示例应用 [![Build Status](https://github.com/spring-projects/spring-petclinic/actions/workflows/maven-build.yml/badge.svg)](https://github.com/spring-projects/spring-petclinic/actions/workflows/maven-build.yml)[![Build Status](https://github.com/spring-projects/spring-petclinic/actions/workflows/gradle-build.yml/badge.svg)](https://github.com/spring-projects/spring-petclinic/actions/workflows/gradle-build.yml)

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/spring-projects/spring-petclinic) [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=7517918)

## 通过几个图表理解 Spring Petclinic 应用

[查看演示文稿](https://speakerdeck.com/michaelisvy/spring-petclinic-sample-application)

## 本地运行 Petclinic

Spring Petclinic 是一个使用 [Maven](https://spring.io/guides/gs/maven/) 或 [Gradle](https://spring.io/guides/gs/gradle/) 构建的 [Spring Boot](https://spring.io/guides/gs/spring-boot) 应用程序。您可以构建 jar 文件并从命令行运行它（Java 17 或更高版本均可）：

```bash
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
./mvnw package
java -jar target/*.jar
```

然后您可以在 <http://localhost:8080/> 访问 Petclinic。

<img width="1042" alt="petclinic-screenshot" src="https://cloud.githubusercontent.com/assets/838318/19727082/2aee6d6c-9b8e-11e6-81fe-e889a5ddfded.png">

或者您可以使用 Spring Boot Maven 插件直接从 Maven 运行它。如果这样做，它将立即接收您在项目中所做的更改（对 Java 源文件的更改也需要编译 - 大多数人为此使用 IDE）：

```bash
./mvnw spring-boot:run
```

> 注意：如果您更喜欢使用 Gradle，可以使用 `./gradlew build` 构建应用程序，并在 `build/libs` 中查找 jar 文件。

## 构建容器

此项目中没有 `Dockerfile`。您可以使用 Spring Boot 构建插件构建容器镜像（如果您有 docker 守护进程）：

```bash
./mvnw spring-boot:build-image
```

## 如果您发现 Spring Petclinic 的 bug/改进建议

我们的问题跟踪器在[这里](https://github.com/spring-projects/spring-petclinic/issues)。

## 数据库配置

在其默认配置中，Petclinic 使用内存数据库 (H2)，该数据库在启动时填充数据。h2 控制台公开在 `http://localhost:8080/h2-console`，可以使用 `jdbc:h2:mem:<uuid>` URL 检查数据库的内容。UUID 在启动时打印到控制台。

如果需要持久数据库配置，则为 MySQL 和 PostgreSQL 提供了类似的设置。请注意，每当数据库类型更改时，应用程序都需要使用不同的配置文件运行：MySQL 使用 `spring.profiles.active=mysql`，PostgreSQL 使用 `spring.profiles.active=postgres`。有关如何设置活动配置文件的更多详细信息，请参阅 [Spring Boot 文档](https://docs.spring.io/spring-boot/how-to/properties-and-configuration.html#howto.properties-and-configuration.set-active-spring-profiles)。

您可以使用适用于您操作系统的任何安装程序在本地启动 MySQL 或 PostgreSQL，或使用 docker：

```bash
docker run -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=petclinic -p 3306:3306 mysql:9.1
```

或

```bash
docker run -e POSTGRES_USER=petclinic -e POSTGRES_PASSWORD=petclinic -e POSTGRES_DB=petclinic -p 5432:5432 postgres:17.0
```

[MySQL](https://github.com/spring-projects/spring-petclinic/blob/main/src/main/resources/db/mysql/petclinic_db_setup_mysql.txt) 和 [PostgreSQL](https://github.com/spring-projects/spring-petclinic/blob/main/src/main/resources/db/postgres/petclinic_db_setup_postgres.txt) 都提供了进一步的文档。

除了原生 `docker`，您还可以使用提供的 `docker-compose.yml` 文件来启动数据库容器。每个容器都有一个以 Spring 配置文件命名的服务：

```bash
docker compose up mysql
```

或

```bash
docker compose up postgres
```

## 测试应用程序

在开发时，我们建议您使用设置为 `main()` 方法的测试应用程序：`PetClinicIntegrationTests`（使用默认的 H2 数据库并添加 Spring Boot Devtools）、`MySqlTestApplication` 和 `PostgresIntegrationTests`。这些设置使您可以在 IDE 中运行应用程序以获得快速反馈，还可以将相同的类作为集成测试针对相应的数据库运行。MySQL 集成测试使用 Testcontainers 在 Docker 容器中启动数据库，Postgres 测试使用 Docker Compose 执行相同的操作。

## 编译 CSS

`src/main/resources/static/resources/css` 中有一个 `petclinic.css`。它是从 `petclinic.scss` 源文件生成的，与 [Bootstrap](https://getbootstrap.com/) 库结合。如果您对 `scss` 进行更改或升级 Bootstrap，则需要使用 Maven 配置文件 "css" 重新编译 CSS 资源，即 `./mvnw package -P css`。Gradle 没有编译 CSS 的构建配置文件。

## 在 IDE 中使用 Petclinic

### 前置要求

您的系统中应安装以下项目：

- Java 17 或更高版本（完整的 JDK，而不是 JRE）
- [Git 命令行工具](https://help.github.com/articles/set-up-git)
- 您首选的 IDE
  - Eclipse 与 m2e 插件。注意：当 m2e 可用时，`Help -> About` 对话框中会有一个 m2 图标。如果 m2e 不存在，请按照[此处](https://www.eclipse.org/m2e/)的安装过程操作
  - [Spring Tools Suite](https://spring.io/tools) (STS)
  - [IntelliJ IDEA](https://www.jetbrains.com/idea/)
  - [VS Code](https://code.visualstudio.com)

### 步骤

1. 在命令行上运行：

    ```bash
    git clone https://github.com/spring-projects/spring-petclinic.git
    ```

1. 在 Eclipse 或 STS 中：

    通过 `File -> Import -> Maven -> Existing Maven project` 打开项目，然后选择克隆仓库的根目录。

    然后在命令行上构建 `./mvnw generate-resources` 或使用 Eclipse 启动器（右键单击项目并选择 `Run As -> Maven install`）来生成 CSS。通过右键单击应用程序的主方法并选择 `Run As -> Java Application` 来运行它。

1. 在 IntelliJ IDEA 中：

    在主菜单中，选择 `File -> Open` 并选择 Petclinic [pom.xml](pom.xml)。点击 `Open` 按钮。

    - CSS 文件是从 Maven 构建生成的。您可以在命令行上构建它们 `./mvnw generate-resources` 或右键单击 `spring-petclinic` 项目，然后选择 `Maven -> Generates sources and Update Folders`。

    - 如果您使用的是最新的 Ultimate 版本，应该已经为您创建了一个名为 `PetClinicApplication` 的运行配置。否则，通过右键单击 `PetClinicApplication` 主类并选择 `Run 'PetClinicApplication'` 来运行应用程序。

1. 导航到 Petclinic

    在浏览器中访问 [http://localhost:8080](http://localhost:8080)。

## 寻找特定内容？

|Spring Boot 配置 | 类或 Java 属性文件  |
|--------------------------|---|
|主类 | [PetClinicApplication](https://github.com/spring-projects/spring-petclinic/blob/main/src/main/java/org/springframework/samples/petclinic/PetClinicApplication.java) |
|属性文件 | [application.properties](https://github.com/spring-projects/spring-petclinic/blob/main/src/main/resources) |
|缓存 | [CacheConfiguration](https://github.com/spring-projects/spring-petclinic/blob/main/src/main/java/org/springframework/samples/petclinic/system/CacheConfiguration.java) |

## 有趣的 Spring Petclinic 分支和分叉

[spring-projects](https://github.com/spring-projects/spring-petclinic) GitHub 组织中的 Spring Petclinic "main" 分支是基于 Spring Boot 和 Thymeleaf 的"规范"实现。在 GitHub 组织 [spring-petclinic](https://github.com/spring-petclinic) 中有[很多分叉](https://spring-petclinic.github.io/docs/forks.html)。如果您有兴趣使用不同的技术栈来实现 Pet Clinic，请加入那里的社区。

## 与其他开源项目的互动

在 Spring Petclinic 应用程序上工作的最好的部分之一是我们有机会与许多开源项目直接接触。我们发现了关于 Spring、Spring Data、Bean Validation 甚至 Eclipse 等各种主题的 bug/建议改进！在许多情况下，它们在短短几天内就被修复/实现了。
以下是其中的列表：

| 名称 | 问题 |
|------|-------|
| Spring JDBC: 简化 NamedParameterJdbcTemplate 的使用 | [SPR-10256](https://github.com/spring-projects/spring-framework/issues/14889) 和 [SPR-10257](https://github.com/spring-projects/spring-framework/issues/14890) |
| Bean Validation / Hibernate Validator: 简化 Maven 依赖项和向后兼容性 |[HV-790](https://hibernate.atlassian.net/browse/HV-790) 和 [HV-792](https://hibernate.atlassian.net/browse/HV-792) |
| Spring Data: 在使用 JPQL 查询时提供更多灵活性 | [DATAJPA-292](https://github.com/spring-projects/spring-data-jpa/issues/704) |

## 贡献

[问题跟踪器](https://github.com/spring-projects/spring-petclinic/issues)是 bug 报告、功能请求和提交拉取请求的首选渠道。

对于拉取请求，编辑器首选项在[编辑器配置](.editorconfig)中可用，便于在常用文本编辑器中使用。在 <https://editorconfig.org> 阅读更多内容并下载插件。所有提交都必须在每个提交消息的末尾包含 __Signed-off-by__ 尾注，以表明贡献者同意开发者来源证书。
有关更多详细信息，请参阅博客文章 [Hello DCO, Goodbye CLA: Simplifying Contributions to Spring](https://spring.io/blog/2025/01/06/hello-dco-goodbye-cla-simplifying-contributions-to-spring)。

## 许可证

Spring PetClinic 示例应用程序根据 [Apache License](https://www.apache.org/licenses/LICENSE-2.0) 2.0 版本发布。
