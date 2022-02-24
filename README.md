# sonar-break-maven-plugin


A maven plugin that will fail a maven build if sonar reports errors with your project.  Tested with SonarQube 5.2 through 8.7.

## Maven 
To include in your project, update your pom.xml with the following:

    <dependencies>
        <dependency>
            <groupId>com.github.phaneesh</groupId>
            <artifactId>sonarbreak</artifactId>
            <version>1.2.7</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>com.github.phaneesh</groupId>
                <artifactId>sonarbreak</artifactId>
                <version>1.2.7</version>
                <configuration>
                    <sonarServer>https://sonar.yourserver.com</sonarServer>
                </configuration>
            </plugin>
        </plugins>
    </build>

### Optional parameters
There are two optional parameters that can be used with this plugin.  
* _sonarLookBackSeconds_: How far into the past the plugin should into sonar for the results of this build (default: 60)
* _waitForProcessingSeconds_: How long to wait for sonar to finish processing the job (default: 300)

These parameter goes into the configuration section so the build piece of your pom.xml would look like: 

    <build>
        <plugins>
            <plugin>
                <groupId>com.github.phaneesh</groupId>
                <artifactId>sonarbreak</artifactId>
                <version>1.2.7</version>
                <configuration>
                    <sonarServer>https://sonar.yourserver.com</sonarServer>
                    <sonarLookBackSeconds>60</sonarLookBackSeconds>
                    <waitForProcessingSeconds>600</waitForProcessingSeconds>
                </configuration>
            </plugin>
        </plugins>
    </build>

You must also have the sonar plugin installed:

    <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>sonar-maven-plugin</artifactId>
        <version>3.9.1.2184</version>
    </plugin>

Then run maven using the command: 

    mvn sonar:sonar sonar-break:sonar-break

Details:
* sonar:sonar - This will execute the sonar task to push the code up to sonar
* sonar-break:sonar-break - This will execute this plugin, which will communicate with your sonar server and will break the build if an error is found.

### Full Example
An full working pom.xml example can be seen in the integration-tests folder here: https://github.com/phaneesh/sonar-break-maven-plugin/blob/master/integration-tests/basic/pom.xml

## Hosting
The plugin is hosted on [Clojars](https://clojars.org/com.github.phaneesh/sonar-break-maven-plugin)

## Development
### Build
You can build and run the tests 
```
mvn clean package
```

### Integration Tests
```
mvn clean install exec:exec -Dmaven.signing.skip=true
```

Integration Test Details:
* Downloads and runs a sonar server
* Builds test poms and pushes the results into sonar
* Tests this plugin by fetching the sonar status

### Skip Signing
If you run "mvn verify" or "mvn install" it will attempt to sign the output using gpg.  Just pass "-Dmaven.signing.skip=true" into maven to skip this plugin.  Example: "mvn clean install exec:exec -Dmaven.signing.skip=true"  


## Version History
* 1.2.7 - Support for Sonarqube 8.x and 9.x and version updates
* 1.2.2 - Suport for SonarQube 6.2 and version updates
* 1.2 - Support for SonarQube 6.0
* 1.1.6 - Switched to Java 8, support for SonarQube 5.6
* 1.1.5 - Upgrading dependencies
* 1.1.4 - Fix for error on first run of project
* 1.1.3 - Support custom sonar key
* 1.1.2 - Upgrading dependencies
* 1.1 - SonarQube 5.3 support
* 1.0 - Initial release
