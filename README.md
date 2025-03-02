Hello world
# maven-unit-and-integration-tests

Sample maven project to run unit and integration tests with maven surefire and maven failsafe plugins.

## Compile and testing the application

Apache maven and Java JDK 8 must be correctly installed on the system. 

In order to compile the application just run:

```shell
mvn clean package
```

To skip unit tests via command line during compilation just add the parameter `-DskipTests=true`:

```shell
mvn clean package -DskipTests=true 
```
Note: More details about the application tests can be found on section [Unit Test and Integration Tests](#unit-tests-and-integration-tests).

To run the application use the following command:
```shell
java -jar target\maven-unit-and-integration-tests-1.0-SNAPSHOT.jar
```

## Unit Tests and Integration Tests

The application is configured with two maven plugins for unit and integration tests.  
In simple words, the `maven-surefire` plugin is designed to run unit tests while the `maven-failsafe` plugin is designed to run integration tests.

This is further explained in Maven FAQ:

* `maven-surefire` plugin is designed for running unit tests and if any of the tests fail then it will fail the build immediately.
* `maven-failsafe` plugin is designed for running integration tests, and decouples failing the build if there are test failures from actually running the tests, thus enabling the post-integration-test phase to execute.

The name "failsafe" was chosen both because it is a synonym of surefire and because it implies that when it fails, it does so in a safe way.

The `maven-failsafe` plugin lifecycle has four phases for running integration tests:  

* `pre-integration-test` for setting up the integration test environment.  
* `integration-test` for running the integration tests.  
* `post-integration-test` for tearing down the integration test environment.  
* `verify` for checking the results of the integration tests.

The `maven-failsafe` plugin has two goals:

* `failsafe:integration-test` runs the integration tests of an application.
* `failsafe:verify` verifies that the integration tests of an application passed.

### Unit Tests

To run the application unit tests just type the following command:
 
```shell
mvn test
```

To test a specific file you can do with:
```
mvn test -Dtest=SampleTest 
``` 

A list of <include> elements specifying the tests (by pattern) should be included in testing on `maven-surefire` plugin. 
When not specified and when the test parameter is not provided, the default includes will be:

```
<includes>
    <include>**/Test*.java</include>
    <include>**/*Test.java</include>
    <include>**/*Tests.java</include>
    <include>**/*TestCase.java</include>
</includes>
```

### Integration Tests

To run the application integration tests just type the following command:

```shell
mvn verify
```

Note: when running `mvn verify` this tell maven to processes all phases of the lifecycle up to `verify` phase.   

To run only the integration tests call the plugin goal directly:

```shell
mvn clean test-compile failsafe:integration-test 
```

or the goal to generate the failsafe reports:

```shell
mvn clean test-compile failsafe:integration-test failsafe:verify
```

A list of <include> elements specifying the test filter (by pattern) should be included in testing on `maven-failsafe` plugin. 
If it is not specified and the test parameter is unspecified as well, the default includes is:
```
<includes>
    <include>**/IT*.java</include>
    <include>**/*IT.java</include>
    <include>**/*ITCase.java</include>
</includes>
```

### Skipping Unit Tests and Integration Tests

To skip all tests (unit and integration tests) when install the package into the local repository via command line add the parameter `-DskipTests=true` (The flag is honored by Surefire, Failsafe and the Compiler Plugin).

```shell
mvn clean install -DskipTests=true 
```

IMPORTANT: The parameter `-DskipTests` only works if the configuration property `<skipTests>` in `maven-surefire` plugin or `maven-failsafe` plugin is not override by another property.  
In our application we've configured the flag `-DskipUTs` on maven surefire plugin and flag `-DskipITs` on maven failsafe plugin to explicit skip unit test and integration tests. 
By default both properties hold the value of `skipTests` if provided by parameter on command line. 

To explicit skip unit tests just add the parameter `-DskipUTs=true`.
```shell
mvn clean install -DskipUTs=true 
```

And to explicit skip integration tests via command line just add the parameter `-DskipITs=true`:

```shell
mvn clean install -DskipITs=true 
```

## References

* https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html
* https://maven.apache.org/surefire/maven-surefire-plugin
* https://maven.apache.org/surefire/maven-failsafe-plugin
