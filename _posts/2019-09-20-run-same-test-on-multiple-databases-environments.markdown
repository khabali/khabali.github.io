---
layout: post
title:  "Run same integration test on multiple databases environments"
date:   2019-09-20 10:56:00 +0200
author: Anas KHABALI
tags: testcontainers junit5 integration-test jdbc docker
comments: true
---
![alt text][idea-capture]{:class="img-responsive"}

Recently at Talend, i have worked on a generic JDBC component able of executing sql queries to read/load data from/to any database supporting JDBC specification
and providing a driver for it.

As a part of this development, I needed to ensure that the component can run correctly with multiples databases.
So, what I needed was to have multiples databases environments and integration tests that can be parameterized and executed on all the environments.

After some research, I have ended up combining **testcontainers** as a database environments provider and **TestTemplate** from **JUnit5**
to handle multiple execution context for the same test and i was satisfied with the end result.

This is what i will be sharing here, but first let's see briefly What are

- [testcontainers](https://www.testcontainers.org/)

>Testcontainers is a Java library that supports JUnit tests, providing lightweight, throwaway instances of common databases,
Selenium web browsers, or anything else that can run in a Docker container.

- [TestTemplate](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-templates)

>A **TestTemplate** method is not a regular test case but rather a template for test cases. As such, it is designed to be invoked multiple times depending on the number of invocation contexts returned by the registered providers. Thus, it must be used in conjunction with a registered TestTemplateInvocationContextProvider extension. Each invocation of a test template method behaves like the execution of a regular **Test** method with full support for the same lifecycle callbacks and extensions. Please refer to Providing Invocation Contexts for Test Templates for usage examples.

## Setup the maven project dependencies

Create a standard maven project and add the required dependencies into the `pom.xml` file of the project.
add the JUnit 5 dependency and the dependency for testcontainers.

````
<properties>
    <testcontainers.version>1.12.1</testcontainers.version>
    <junit.jupiter.version>5.5.2</junit.jupiter.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>${junit.jupiter.version}</version>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.testcontainers</groupId>
        <artifactId>testcontainers</artifactId>
        <version>${testcontainers.version}</version>
        <scope>test</scope>
    </dependency>
</dependencies>
````
For every database we will need to add the container dependency with it required JDBC driver.
Here are the dependencies for MySQL, Postgres MariaDB and MSSQL that we will be using here.


>You can check [the available database containers](https://www.testcontainers.org/modules/databases/) provided by testcontainers
or check how to create a [custom one](https://www.testcontainers.org/features/creating_container/).

````
<dependencies>
      <dependency>
           <groupId>org.testcontainers</groupId>
           <artifactId>mysql</artifactId>
           <version>${testcontainers.version}</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.17</version>
           <scope>test</scope>
       </dependency>

       <dependency>
           <groupId>org.testcontainers</groupId>
           <artifactId>postgresql</artifactId>
           <version>${testcontainers.version}</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.postgresql</groupId>
           <artifactId>postgresql</artifactId>
           <version>42.2.8</version>
           <scope>test</scope>
       </dependency>

       <dependency>
           <groupId>org.testcontainers</groupId>
           <artifactId>mariadb</artifactId>
           <version>${testcontainers.version}</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mariadb.jdbc</groupId>
           <artifactId>mariadb-java-client</artifactId>
           <version>2.3.0</version>
           <scope>test</scope>
       </dependency>

       <dependency>
           <groupId>org.testcontainers</groupId>
           <artifactId>mssqlserver</artifactId>
           <version>${testcontainers.version}</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>com.microsoft.sqlserver</groupId>
           <artifactId>mssql-jdbc</artifactId>
           <version>7.0.0.jre8</version>
           <scope>provided</scope>
       </dependency>
</dependencies>
````

The project dependencies are setup. Let's write some Java code.

## Test Template Invocation Context Provider

To work with test template in JUnit 5. we will need to implement  *TestTemplateInvocationContextProvider* interface which will provides the invocation context
to tests.

In the test sources folder we create *DatabaseInvocationContextProvider* class that implement the *TestTemplateInvocationContextProvider*
and handle the database containers downloads, start and stop.

Let's walk through the code to see how we can handle that.

````
package io.github.khabali;

import org.junit.jupiter.api.extension.AfterAllCallback;
import org.junit.jupiter.api.extension.BeforeEachCallback;
import org.junit.jupiter.api.extension.Extension;
import org.junit.jupiter.api.extension.ExtensionContext;
import org.junit.jupiter.api.extension.ParameterContext;
import org.junit.jupiter.api.extension.ParameterResolver;
import org.junit.jupiter.api.extension.TestTemplateInvocationContext;
import org.junit.jupiter.api.extension.TestTemplateInvocationContextProvider;
import org.testcontainers.containers.JdbcDatabaseContainer;
import org.testcontainers.containers.MSSQLServerContainer;
import org.testcontainers.containers.MariaDBContainer;
import org.testcontainers.containers.MySQLContainer;
import org.testcontainers.containers.PostgreSQLContainer;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Stream;

import static java.util.Arrays.asList;

class DatabaseInvocationContextProvider implements TestTemplateInvocationContextProvider{

    private final Map<String, JdbcDatabaseContainer> containers;

    public DatabaseInvocationContextProvider() {
      containers = new HashMap<>();
      containers.put("postgresql", new PostgreSQLContainer("postgres:11.1"));
      containers.put("mysql", (MySQLContainer) new MySQLContainer("mysql:8.0.13")
                .withCommand("--default-authentication-plugin=mysql_native_password"));
      containers.put("mariadb", new MariaDBContainer("mariadb:10.4.0"));
      containers.put("mssql", new MSSQLServerContainer("mcr.microsoft.com/mssql/server:2017-CU12"));
    }

    @Override
    public boolean supportsTestTemplate(ExtensionContext extensionContext) {
        return true;
    }

    @Override
    public Stream<TestTemplateInvocationContext> provideTestTemplateInvocationContexts(ExtensionContext extensionContext) {
        return containers.keySet().stream().map(this::invocationContext);
    }

    private TestTemplateInvocationContext invocationContext(final String database) {
        return new TestTemplateInvocationContext() {

            @Override
            public String getDisplayName(int invocationIndex) {
              return database;
            }

            @Override
            public List<Extension> getAdditionalExtensions() {
              final JdbcDatabaseContainer databaseContainer = containers.get(database);
              return asList(
                  (BeforeEachCallback) context -> databaseContainer.start(),
                  (AfterAllCallback) context -> databaseContainer.stop(),
                  new ParameterResolver() {

                      @Override
                      public boolean supportsParameter(ParameterContext parameterCtx,
                                                       ExtensionContext extensionCtx) {
                          return parameterCtx.getParameter().getType()
                                       .equals(JdbcDatabaseContainer.class);
                      }

                      @Override
                      public Object resolveParameter(ParameterContext parameterCtx,
                                                     ExtensionContext extensionCtx) {
                          return databaseContainer;
                      }
                  });
            }
        };
    }
}
````

I create a containers map indexed by the database name what I want to run tests on.

>**Note** that every time a database container is instantiated. It will download the specified docker image for the targeted database from [Docker Hub](https://hub.docker.com/) if it's not already present locally. **This is done once**, Then, the image will be available locally for next execution like any other docker image on your machine.

Then I will need to implements two methods of *TestTemplateInvocationContextProvider* interface.

- *supportsTestTemplate()* : determine if this provider supports providing invocation contexts for the test template method represented by the supplied context. We just return true here as we assume that any test extended with this extension can use it.
- *provideTestTemplateInvocationContexts()* : provide invocation contexts for the test template method represented by the supplied context.
This method is only called by the framework if supportsTestTemplate previously returned true for the same ExtensionContext. Thus, this method must not return an empty Stream. Here we will create *TestTemplateInvocationContext* instance for every databases.

In *TestTemplateInvocationContext* we add 3 extensions :

- *BeforeEachCallback* where we will start the container
- *AfterAllCallback* where we will stop the container after the test execution
- *ParameterResolver* where we will provide the container instance as a parameter that can be used as a test parameter.


Let's write a simple test template and use this extension.

## Test Template

````
package io.github.khabali;

import org.junit.jupiter.api.TestTemplate;
import org.junit.jupiter.api.extension.ExtendWith;
import org.testcontainers.containers.JdbcDatabaseContainer;

import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;

@ExtendWith(DatabaseInvocationContextProvider.class)
class MyIntegrationTest {

    @TestTemplate
    void myAwesomeTest(JdbcDatabaseContainer database) {

        // write test relying on the database you get as an argument
        assertTrue(database.isRunning());
        assertNotNull(database.getJdbcUrl());
        assertNotNull(database.getUsername());
        assertNotNull(database.getPassword());
        assertNotNull(database.getDriverClassName());

    }
}
````
Here I have a simple JUnit5 test extended by *DatabaseInvocationContextProvider* that will provide the execution contexts for the test template.
Note that the test template get a parameter *JdbcDatabaseContainer* which make the databases information available in the test.

From here, I can passe those information to my component to test the program logic.

>For **MSSQL** a usage license needs to be added to the project for that you will need to add a *container-license-acceptance.txt* file to
the test resources folder with this line `mcr.microsoft.com/mssql/server:2017-CU12`

Run this test to check that the *myAwesomeTest* is executed 4 times with the adequate parameter.

This was very useful to test and ensure that my component works as expected on different databases without writing specific tests for every databases.

The complete project can be found in this github project [testcontainers-junit5](https://github.com/khabali/testcontainers-junit5)


[idea-capture]: {{site.baseurl}}/assets/images/2019/10/20/idea_testcontainer_testtemplate_junit5_zoomed.png "Test execution result"   
