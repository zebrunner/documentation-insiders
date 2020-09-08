# Advanced reporting

## Logger

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

## Screenshots

In case you are using TestNG as a UI testing framework it might come handy to have an ability to track captured screenshots in scope of Zebrunner reporting. Agent comes with a Java API allowing you to send your screenshots to Zebrunner so they will be attached to test run. Below is a sample code of test sending screenshot to Zebrunner:

``` java
import com.zebrunner.agent.core.registrar.Screenshot;
import org.testng.annotations.Test;

public class AwesomeTests {

    @Test
    public void myAwesomeTest() {
        // capture screenshot 
        Screenshot.upload(screenshotBytes, capturedAtMillis);
        // meaningful assertions
    }

}
```

!!! note ""
    Screenshot should be passed as byte array along with unix timestamp in milliseconds corresponding to the moment when screenshot was captured. 
    If `null` is supplied instead of timestamp - it will be generated automatically, however it is strongly recommended to include accurate timestamp in order to get accurate tracking.

## Ownership

You might want to add transparency to the process of automation maintenance by having an engineer responsible for evolution of specific tests or test classes. Zebrunner comes with a concept of maintainer - a person that can be assigned to maintain tests. In order to keep track of those agent comes with @Maintainer annotation. Test classes and methods can be annotated. It is also possible to override class-level maintainer on a mehtod-level. See a sample test class below:

``` java
import com.zebrunner.agent.core.reporting.Maintainer;
import org.testng.annotations.Test;

@Maintainer("kenobi")
public class AwesomeTests {

    @Test
    @Maintainer("skywalker")
    public void awesomeTest() {
        // meaningful assertions
    }

    @Test
    public void anotherAwesomeTest() {
        // meaningful assertions
    }
}
```

!!! note ""
    In the example above `kenobi` will be reported as a maintaner of `anotherAwesomeTest` (class-level value taken into account), while `skywalker` will be reported as a mainainer of test `awesomeTest`. Maintainer username should be valid Zebrunner username, otherwise it will be set to anonymous.
