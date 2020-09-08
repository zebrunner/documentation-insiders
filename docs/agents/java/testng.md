# TestNG agent

!!! note ""
    Agent supports TestNG version starting from 7.0.0

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

1. Environment variables 
2. Program arguments 
3. YAML file
4. Properties file

!!! note ""
    Configuration lookup will be performed in order listed above, meaning that environment configuration will always take precedence over YAML and so on. It is also possible to override configuration parameters by supplying them via configuration provider having higher precedence. Once configuration is in place agent is ready to track you test run events, no additional configuration required.
 

=== "Environment variables"

    ``` properties
    REPORTING_ENABLED=true
    REPORTING_SERVER_HOSTNAME=localhost:8080
    REPORTING_SERVER_ACCESS_TOKEN-token=<token>
    ``` 
    
=== "Program arguments"

    ``` properties
    -Dreporting.enabled=true
    -Dreporting.server.hostname=localhost:8080
    -Dreporting.server.access-token=<token>
    ```
    
=== "agent.yaml"

    ``` yaml
    reporting:
    enabled: true
    server:
      hostname: localhost:8080/api
      access-token: <token>
    ```

=== "agent.properties"

    ``` properties
    reporting.enabled=true
    reporting.server.hostname=localhost:8080
    reporting.server.access-token=<token>
    ```
    
!!! note ""
    Access token and hostname are available in [User profile](../../guide/user_profile.md). Configuration files `agent.yaml` or `agent.properties` should be placed into project resource folder.

## Step 3: Advanced reporting

Read the following docs to enabled advanced reporting features:

* [Configure loggers](advanced.md#loggers)
* [Capture screenshots](advanced.md#screenshots)
* [Track test ownership](advanced.md#ownership)
