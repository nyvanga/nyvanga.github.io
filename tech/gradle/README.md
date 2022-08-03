# Gradle

Gradle is (a lot) better than Maven.

- [Gradle User Manual](https://docs.gradle.org/current/userguide/userguide.html)
- [Gradle Build Language Reference](https://docs.gradle.org/current/dsl/index.html)

## Problem: Gradle uses all 32 threads of your CPU, and that hurts...

No problem. Set the max workers option.

On the command-line:
```
./gradlew --max-workers 8 some-massivly-parrallel-gradle-task
```

In the `${HOME}/.gradle/gradle.properties` file:
```
org.gradle.workers.max=8
```

## Plugins

- [Java plugin](https://docs.gradle.org/current/userguide/java_plugin.html) ([dependency configurations](https://docs.gradle.org/current/userguide/java_plugin.html#tab:configurations))
- [Java library plugin](https://docs.gradle.org/current/userguide/java_library_plugin.html) ([configurations](https://docs.gradle.org/current/userguide/java_library_plugin.html#sec:java_library_configurations_graph))
- [War plugin](https://docs.gradle.org/current/userguide/war_plugin.html) ([dependency management](https://docs.gradle.org/current/userguide/war_plugin.html#sec:war_dependency_management))

## Maven scopes vs. gradle configurations

[Source](https://reflectoring.io/maven-scopes-gradle-configurations/)

Maven scopes don’t translate perfectly to Gradle configurations because Gradle configurations are more granular. However, here’s a table that translates between Maven scopes and Gradle configurations with a few notes about differences:

Maven Scope | Equivalent Gradle Configuration
------------|--------------------------------
`compile`   | `api` if the dependency should be exposed to consumers, `implementation` if not
`provided`  | `compileOnly` (note that the `provided` Maven scope is also available at runtime while the `compileOnly` Gradle configuration is not)
`runtime`   | `runtimeOnly`
`test`      | `testImplementation`