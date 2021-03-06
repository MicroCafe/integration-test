## How to Automate Integration Test Suite using SoapUI

## Background

It is very easy when it comes to automating unit tests or UI tests. There are plenty of tools and techniques for it. Let's say:

•   You need to automate your JUnit test with real code or any mock service. It can be easily combined with the build system, e.g. one can use an Ant/Maven task for running JUnit test cases and generate a report out of it.

•   You need to automate your UI test. One can use Selenium scripts and add them to the CI system.

The challenge comes when you need to test your backend service with an end-to-end flow without using any frontend technology or any client.

## Solution

The solution is simple yet tricky. You need not write a test case framework or a test base for running integration tests for web services.

The most famous and straightforward technique, which you may already know, to test your web services (both SOAP and REST, though it supports other protocols and mechanisms like JMS and MQTT) is to use a test client like ARC or Postman, and last but not least, SOAP UI.

Each of them has its own benefits, but as per my personal experience and that of my peers, I like SOAP UI the most (moving forward, I will focus on SOAP UI only).

To automate the process, first we first need to do it manually, and then invoke it using an automated system. You can follow these steps, each of them shown in screenshots:

•   Create a SOAP UI Test Project.

![](https://dzone.com/storage/temp/6660230-step12-create-project.png)!

•   Define your endpoints.

![](https://dzone.com/storage/temp/6660232-step23-define-endpoint3.png)!

•   Create a Test Suite and Test Case.

![](https://dzone.com/storage/temp/6660233-step3-test-suit-case.png)!

•   Add a Test Step for your endpoint.

![](https://dzone.com/storage/temp/6660234-step4-test-step.png)!

•   Generate and save the project descriptor.

![](https://dzone.com/storage/temp/6660235-step5-generate-project.png)!

Now you need to generate an automated script to run the suite created with the help of SOAP UI in the above steps. For this, you can use a tool like Ant or Maven.

•   Create a test project add Maven support to it.

•   Add the following plugin to your pom.xml:

```
<plugin> 
    <groupId>com.smartbear.soapui</groupId>
    <artifactId>soapui-maven-plugin</artifactId>
    <version>5.2.1</version>
    <configuration>
        <projectFile>${basedir}/soapui-project.xml</projectFile>
    </configuration>
    <executions>
        <execution>
            <id>soapui-test</id>
            <phase>integration-test</phase>
            <goals>
              <goal>test</goal>
            </goals>
          </execution>
      </executions>
</plugin>

```

You would need to add the following repository because it not available under the default Maven repo:

```

<pluginRepositories>
    <pluginRepository>
        <id>smartbear-sweden-plugin-repository</id>
        <url>http://www.soapui.org/repository/maven2</url>
    </pluginRepository>
</pluginRepositories>

```

Voila! That's it!

Now you are ready to run the automated integration test for your web service.

Run the following build command and see the magic:

```
mvn clean integration-test

```

You can refer the 

https://github.com/ers-hcl/integration-test-server

for setting up a test server
