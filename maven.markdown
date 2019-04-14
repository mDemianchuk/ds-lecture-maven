## Java Build Tools
Build tools are programs that automate the creation of executable applications from source code.

`TODO: add more content`

Typical Java project consists of such tasks:
- Retrieving and resolving dependencies
- Adding necessary jars to a classpath
- Compiling the project's source code into binary code
- Running tests
- Packaging the compiled code into deployable artifacts
- Deploying these artifacts to production systems such as an application server or remote repository

### Advantages
The advantages of utilizing build tools in Java projects include:
- An important prerequisite for continuous integration and continuous testing
- Improved quality of the end product
- Acceleration of the compile processing
- Elimination of redundant tasks
- History of builds and releases make the investigation and tracking of issues easier


### Variety of Java Build Tools
The most widely used build tools for Java projects:
- [Apache Maven](#apache-maven)
- [Gradle](#gradle)
- [Other build tools](#other-build-tools)

## Apache Maven
![alt text](images/maven-logo.png)

**Apache Maven** is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting, and documentation from a central piece of information.
https://maven.apache.org/

Apache Maven automates build tasks to minimize the risk of human error while building the software manually.


### Maven's key advantages
- **Easy project setup:** With Maven, users have an ability to create a project from a template (also known as an archetype). It reduces an overhead with manual configuration of a project and eliminates human error.

- **Extensibility:** With Maven, users have an ability to easily write plugins in Java or scripting languages.

- **Dependency management:** Maven is capable of automatic updating, downloading, and validating the compatibility of the dependencies, as well as reporting the dependency closures (also known as transitive dependencies).

- **Isolation between project dependencies and plugins:** In Maven project, the dependencies and plugins are retrieved from separate repositories, which reduces the number of conflicts when plugins require additional dependencies.

- **Central repository system:** Maven can either load the project dependencies from the local or remote central repository, such as [Maven Central](https://search.maven.org/classic/). It allows users of Maven to reuse JARs across projects and encourages communication between projects to ensure that issues with backward compatibility are dealt with.

- **Multi-module projects support:** In Maven, the mechanism that handles multi-module projects (also called aggregator projects) is called *Reactor*. The Reactor retrieves all required modules to build, then organizes their builds into the correct order, and finally, builds them one at the time.

- **Release management:** Without extra configuration, Maven integrates with the version control system and manages the project's release based on a certain tag.

- **Coherent site of project information:** Using the same metadata as for the build process, Maven is able to generate a web site or PDF including any relevant documentation and reports about the current state of the project.

### Project Object Model
A Maven project relies on a concept of *Project Object Model (POM)*, which is represented by a *pom.xml* file. This file describes the project, takes care of dependency management, and configures necessary plugins for building the software.

In multi-module projects, the *pom.xml* defines the relationships between modules. An example of *pom.xml* file is shown below:

~~~
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>edu.luc</groupId>
  <artifactId>pom-example</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>

  <name>pom-example</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
</project>
~~~

#### Project Identifiers
In order to uniquely identify a project and specify its packaging, Maven uses a number of identifiers (also known as coordinates):

- **groupId** – a unique group or company name
- **artifactId** – a unique name of the project
- **version** – a version of the project
- **packaging** – a packaging method to generate artifacts (e.g. *war/jar/zip*)

A combination of *groupId*, *artifactId*, and *version* defines the unique identifier for the project. This set of coordinates allows a user to specify which versions of external libraries will be utilized in the project.

#### Dependencies
Dependencies represent external libraries that the project is dependent on. This Maven's feature automatically downloads those external jars from a remote repository in case a user does not have them in the local repository.

Dependencies provide the following benefits:
- less storage usage by eliminating unnecessary downloads from remote repositories
- significantly faster project's builds
- an efficient mechanism for managing and exchanging artifacts within a group or company (those artifacts need to be built from source only once)

If a project is dependent on an external library, a user needs to specify the *groupId*, *artifactId*, and the *version* of this artifact. An example is seen below:

~~~
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>27.1-jre</version>
</dependency>
~~~

####  Repositories
In Maven, a repository stores built dependencies and artifacts of various types. The default local repository can be found in the *.m2/repository* folder under the user's home directory.

When resolving dependencies, Maven checks for plugins and artifacts in the local repository. In case they are not available there, it downloads them from a remote cenral repository, such as [Maven Central](https://search.maven.org/classic/) and saves them in the local repository.

#### Build
Even though Maven's *build* is one of the optional sections of the *POM*, it provides a variety of project's configurations. In this section a user can set the default Maven *goal*, specify the folder for compiled artifacts (by default named *target*), and define the project's final name. An example of the *build* section is shown below:

~~~
<build>
    <defaultGoal>install</defaultGoal>
    <directory>${basedir}/target</directory>
    <finalName>${artifactId}-${version}</finalName>
    <filters>
      <filter>filters/filter1.properties</filter>
    </filters>
    //...
</build>
~~~

### Maven Build Lifecycles
In Maven, the *build* follows a specified set of rules named *lifecycle*. It lets a user execute individual *lifecycle phases*.

#### Lifecycle Phases
The list of the most important lifecycle phases:
- **validate** – validates the correctness of the project
- **compile** – compiles the source code into binary artifacts
- **test** – executes unit tests
- **package** – packages compiled code into an archive file such as *war*/*jar*/*zip*
- **integration-test** – runs intgration tests
- **verify** – validates the package
- **install** – installs the package into the local repository
- **deploy** – deploys the package to a remote server or repository

#### Plugins and Goals

A Maven *plugin* is a collection of one or more *goals*. *Goals* are executed in *phases*, which helps to determine the order in which the *goals* are executed.

The full list of officially supported plugins can be found on [Maven's website](https://maven.apache.org/plugins/).

A command to execute a *lifecycle phase* (including all of the prior *phases*) is shown below:

`mvn <phase>`

For example, *mvn package* executes each default *lifecycle phase* in such order:
1. *validate*
2. *compile*
3. *test*
4. *package*

### Maven Project Example
This section will show you how to create a Maven project using the command line interface.

#### Generating a Java Project
The following **in-line** command will generate a simple Java project from a Maven archetype:

~~~
mvn archetype:generate
  -DgroupId=edu.luc
  -DartifactId=simple-project
  -DarchetypeArtifactId=maven-archetype-quickstart
  -DinteractiveMode=false
~~~

The *groupId* coordinate specifies the group, company, or individual that created a project. This identifier should follow [Java's package name rules](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7), which means it should start with a reversed company domain. For example, `org.apache.maven`, `org.apache.commons`. However, Maven does not enforse this rule.
https://maven.apache.org/guides/mini/guide-naming-conventions.html

The base package name used in the project is specified by *artifactId*. The only restriction that applies to this identifier is that it should be lowercase.

As we did not specify the *version* and the *packaging* type, these will be set to default values — the *version* will be set to *1.0-SNAPSHOT*, and the *packaging* will be set to *jar*.

By setting `interactiveMode=true` we will make Maven ask for all of the required parameters.

By execution of the command, Maven will generate a simple Java project. If we navigate to *src/main/java* directory, we will see an *App.java* class, which is a well-known *"Hello World"* program. The command also generates a test class in *src/test/java* directory and its *pom.xml* will look as follows:

~~~
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>edu.luc</groupId>
  <artifactId>simple-project</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>simple-project</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
~~~

By default, Maven also generates the *junit* dependecy.

#### Compiling and Packaging a Project
In order to compile our project, we need to run the following command:

`mvn compile`

Maven will execute every lifecycle phase prior to *compile* phase to generate the project's source files. To run only the *test* phase, we can execute the next command:

`mvn test`

Running the following command will produce the compiled jar file:

`mvn package`

#### Executing an Application
In order to execute the application, we need to add and configure the required *exec-maven-plugin* in the project's *pom.xml*:

~~~
<build>
    <sourceDirectory>src</sourceDirectory>
    <plugins>
        <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.5.0</version>
            <configuration>
                <mainClass>edu.luc.simple-project.App</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
~~~
The *maven-compiler-plugin* will take care of compiling the source code using Java *version 1.8*.

The following command will execute the application:

`mvn exec:java`

## Gradle
![alt text](images/gradle-logo.png)

## Other Build Tools
Other available Java build tools:
- [Apache Ant](https://ant.apache.org/)
- [Jerkar](http://project.jerkar.org/)

## Citations
