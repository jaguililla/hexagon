
# Module hexagon_core

This module holds utilities used in other libraries of the toolkit. Check the packages'
documentation for more details. You can find a quick recap of the main features in the sections
below.

### Install the Dependency

This module is not meant to be imported directly. It will be included by using any other part of the
toolkit. However, if you only want to use the utilities, logging or dependency injection (i.e.: for
a desktop application), you can import it with the following code:

=== "build.gradle"

    ```groovy
    repositories {
        mavenCentral()
    }

    implementation("com.hexagonkt:hexagon_core:$hexagonVersion")
    ```

=== "pom.xml"

    ```xml
    <dependency>
      <groupId>com.hexagonkt</groupId>
      <artifactId>hexagon_core</artifactId>
      <version>$hexagonVersion</version>
    </dependency>
    ```

### Logger

The following code block shows the most common use cases for the [Logger] class:

@code hexagon_core/src/test/kotlin/HexagonCoreSamplesTest.kt:logger

By default, Hexagon uses the [Java Util Logging] logging library, you can use any of its
implementations by just adding another logging adapter as a dependency. Below you can see some
alternatives:

[JUL]: https://docs.oracle.com/javase/8/docs/api/java/util/logging/package-summary.html

=== "build.gradle"

    ```groovy
    implementation("com.hexagonkt:logging_logback:$hexagonVersion") // Full featured implementation
    ```

=== "pom.xml"

    ```xml
    <!--
     ! Pick ONLY ONE of the options below
     !-->
    <!-- Full featured implementation -->
    <dependency>
      <groupId>com.hexagonkt</groupId>
      <artifactId>logging_logback</artifactId>
      <version>$hexagonVersion</version>
    </dependency>
    ```

!!! Info
    The above adapters bridge other logging libraries (that may be used by other third party
    libraries you use (if you want to disable this behaviour, you need to explicitly exclude
    bridging libraries).

=== "build.gradle"

    ```groovy
    // Bridges
    runtimeOnly("org.slf4j:jcl-over-slf4j:1.7.30")
    runtimeOnly("org.slf4j:log4j-over-slf4j:1.7.30")
    runtimeOnly("org.slf4j:jul-to-slf4j:1.7.30") // Don't add it if you are using 'slf4j-jdk14'
    ```

=== "pom.xml"

    ```xml
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>jcl-over-slf4j</artifactId>
      <version>1.7.30</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>log4j-over-slf4j</artifactId>
      <version>1.7.30</version>
    </dependency>
    <!-- Don't add the next one if you are using 'slf4j-jdk14' -->
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>jul-to-slf4j</artifactId>
      <version>1.7.30</version>
    </dependency>
    ```

[Logger]: com.hexagonkt.logging/-logger/index.md

### Dependency injection

You can take advantage of dependency injection using the [InjectionManager] object.

The implementation is a map of classes (with an optional tag) to provider functions (in essence:
`Map<KClass<*>, () -> Any>`). It is a very simple, yet complete, DI implementation.

You can bind supplier functions or objects to classes. If a class is already bound, later calls to
`bind*` methods are ignored. However, you can use the `forceBind*` methods if you need to override
a binding (in tests for example).

Check this sample to bind constructor functions or objects to classes, and inject them later:

@code hexagon_core/src/test/kotlin/HexagonCoreSamplesTest.kt:injectionUsage

!!! Info
    Dependency Injection is not required by the toolkit. All classes and methods have versions
    receiving all of their dependencies, so you can use them instead relying on injection (or use
    another DI library of your choice).

[InjectionManager]: com.hexagonkt.injection/-injection-manager/index.md

### Serialization

The core module has utilities to serialize/parse data classes to JSON and YAML. Read the following
snippet for details:

@code hexagon_core/src/test/kotlin/HexagonCoreSamplesTest.kt:serializationUsage

# Package com.hexagonkt.helpers

JVM information, a logger class and other useful utilities.

# Package com.hexagonkt.injection

Utilities to bind classes to creation closures or instances, and inject instances of those classes
later.

# Package com.hexagonkt.logging

Provides a logging management capabilities abstracting the application from logging libraries.

# Package com.hexagonkt.serialization

Parse/serialize data in different formats to class instances.
