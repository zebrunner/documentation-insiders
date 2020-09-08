# TestNG (7.0.0+) agent

Official Zebrunner TestNG agent providing reporting and smart reruns functionality. In order to enable Zebrunner Listener for TestNG no special configuration is required - service discovery mechanism will automatically register listener once it will be availlable on your test application classpath.

**Step1: Add project dependency**

Agent comes bundled with TestNG 7.1.0, so you may want to comment our your dependency or exclude it from agent.

=== "Mavan"

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

**Step 2: Configure agent**

There are multiple ways to provide agent configuration:

* Environment variables
* Program arguments
* YAML file
* Properties file

!!! note ""
    Configuration lookup will be performed in order listed above, meaning that environment configuration will always take precedence over YAML and so on. It is also possible to override configuration parameters by supplying them via configuration provider having higher precedence. Once configuration is in place agent is ready to track you test run events, no additional configuration required.

