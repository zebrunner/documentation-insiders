# TestNG agent

!!! note ""
    Agent support TestNG version starting from 7.0.0.

Official Zebrunner TestNG agent providing reporting and smart reruns functionality. In order to enable Zebrunner Listener for TestNG no special configuration is required - service discovery mechanism will automatically register listener once it will be availlable on your test application classpath.

## Step1: Add project dependency

Agent comes bundled with TestNG 7.1.0, so you may want to comment our your dependency or exclude it from agent.

=== "Maven"

    ``` xml
    <dependency>
      <groupId>com.zebrunner</groupId>
      <artifactId>agent-testng</artifactId>
      <version>1.0.0</version>
    </dependency>
    ```

=== "Gradle"

    ``` gradle
    dependencies {
      testImplementation 'com.zebrunner:agent-testng:1.0.0'
    }
    ```

## Step 2: Configure agent

There are multiple ways to provide agent configuration:

* Properties file
* YAML file
* Program arguments
* Env variables

!!! note ""
    Configuration lookup will be performed in order listed above, meaning that environment configuration will always take precedence over YAML and so on. It is also possible to override configuration parameters by supplying them via configuration provider having higher precedence. Once configuration is in place agent is ready to track you test run events, no additional configuration required.
    
=== "agent.properties"

    ``` properties
    reporting.enabled=true
    reporting.server.hostname=localhost:8080
    reporting.server.access-token=<token>
    ```

=== "agent.yaml"

    ``` yaml
    reporting:
    enabled: true
    server:
      hostname: localhost:8080/api
      access-token: <token>
    ```
    

=== "Program arguments"

    ``` properties
    -Dreporting.enabled=true
    -Dreporting.server.hostname=localhost:8080
    -Dreporting.server.access-token=<token>
    ```


=== "Env variables"

    ``` properties
    REPORTING_ENABLED=true
    REPORTING_SERVER_HOSTNAME=localhost:8080
    REPORTING_SERVER_ACCESS_TOKEN-token=<token>
    ```

## Step 3: Configure logger

It is also possible to enable log collection for your tests. Currently three logging frameworks are supported out of the box: logback, log4j, log4j2. In order to enable logging all you have to do is register reporting appender in your test framework configuration file.

=== "Logback"

    Add dependency:

    ``` xml
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
    </dependency>
    ```
    
    Add logback.xml to resources folder:
    
    ``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
        <appender name="ReportingAppender" class="com.zebrunner.agent.core.appender.logback.ReportingAppender">
           <encoder>
              <pattern>%d{HH:mm:ss.SSS} [%t] %-5level - %msg%n</pattern>
           </encoder>
        </appender>
        <root level="info">
            <appender-ref ref="ReportingAppender" />
        </root>
    </configuration>
    ```
    
    Sample logger usage:
    
    ``` java
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    public class AwesomeTests {

        private static final Logger LOGGER  = LoggerFactory.getLogger(AwesomeTests.class);

        @Test
        public void awesomeTest() {
            LOGGER.info("Test info");
        }
    }
    ```
    

=== "Log4j"

    Add dependency:

    ``` xml
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    ```
    
    Add log4j.properties to resources folder:
    
    ``` properties
    log4j.rootLogger = INFO, zebrunner
    log4j.appender.zebrunner=com.zebrunner.agent.core.appender.log4j.ReportingAppender
    log4j.appender.zebrunner.layout=org.apache.log4j.PatternLayout
    log4j.appender.zebrunner.layout.conversionPattern=pattern">[%d{HH:mm:ss}] %-5p (%F:%L) - %m%n
    ```
    
    Sample logger usage:
    
    ``` java
    import org.apache.log4j.Logger;

    public class AwesomeTests {

        private Logger LOGGER = Logger.getLogger(AwesomeTests.class);

        @Test
        public void awesomeTest() {
            LOGGER.info("Test info");
        }
    }
    ```

=== "Log4j2"

    Add dependency:

    ``` xml
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.13.3</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.13.3</version>
    </dependency>
    ```
    
    Add log4j2.xml to resources folder:
    
    ``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration packages="com.zebrunner.agent.core.appender.log4j2">
       <properties>
          <property name="pattern">[%d{HH:mm:ss}] %-5p (%F:%L) - %m%n</property>
       </properties>
       <appenders>
          <ReportingAppender name="ReportingAppender">
             <PatternLayout pattern="${pattern}" />
          </ReportingAppender>
       </appenders>
       <loggers>
          <root level="info">
             <appender-ref ref="ReportingAppender"/>
          </root>
       </loggers>
    </configuration>
    ```
    
    Sample logger usage:
    
    ``` java
    import org.apache.logging.log4j.LogManager;
    import org.apache.logging.log4j.Logger;

    public class AwesomeTests {

        private Logger LOGGER = LogManager.getLogger(AwesomeTests.class);;

        @Test
        public void awesomeTest() {
            LOGGER.info("Test info");
        }
    }
    ```
