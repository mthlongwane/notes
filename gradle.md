# 🚀 Experimenting with Gradle

In this file I document my notes and learnings on the Gradle build tool.

## Overview

Build automation tools like gradle take care of the following tasks:
1. Dependency Management - Automating the downloading of dependencies and transitive dependencies for a project.
2. Build configuration - Configuration for builing the project, such as source code  location
3. Tasks - Compiling Testing and Publishing 

Note: check out Gradle for Build Engineers course.

## Projects and sub-projects 

Gradle has 2 types of configurations. Settings and the Project build configuration.

The entire project is referred to as the root project and each of the modules/components of the project are reffered to as sub-projects.
There might be some global configuration for the entire project. The settings file would contain this. 
Each subproject may need to be built with different steps therefore there will be a different build configuration file in each subdirectory.

## Gradle wrapper
The Gradle wrapper ensures consistent and reproducible builds.
The Gradle wrapper uses the gradle-wrapper.properties file to run a script that will download the version of gradle specified in the project so that all builds are consistent. 
Gradle wrapper versions are either suffixed with a "-all" or "-bin".
-  "-all" -  binary & sources
- "-bin" - binary only (smaller in size and reccomended)

## Gradle Plugins
Plugins provide reusable common functionality. When plugins are used, we say that the plugin is being "applied". Plugins can be applied to the Settings file at the root of the project as well as in the sub project build files. 
Plugins applied to a project provide three main capabilities:
1. Plugins can add a new configuration model for which values can be specified.  i.e
``` 
    // This is an example of a plugin being applied to a build config file.
    plugins{
        id("com.gradle.someplugin")
    }
        newConfigAvailableAfterApplyingPlugin{
        someConfigValues = "value is specified"
    }
```
2. Plugins may initialise the new configuration to default values. 
3. Plugins can add functionality by making new tasks available.
```
    ./gradlew newTaskAvailabelAfterApplyingPlugin
```

There are three types of plugins. Core, Community and Local.
- Core - Are shipped with the gradle tool binary [here](https://docs.gradle.org/current/userguide/plugin_reference.html)
- Community - Created by members of the Gradle Build Tool Community. When working with community plugins you need to specify the version you are using. With Core plugins you do not need to specify the version you are using since Gradle Build Tool will use the version available in the Binary.
- Local  -  Custom plugins for common functionality specific to a project that might not make sense to share with others.  

Example: Core Java Plugins
- Base Java Plugin 
    - Adds new build configuration i.e source code locations: SourceSet. SourceSets are defaulted to "src/main/java" and "src/test/java"
    - Adds Tasks such as "compileJava" and "test"

- Java Library Plugin (useful if you are building a library that will be published)
    - Automatically applies the Base Java plugin
    - Adds "api" dependency management configuration

- Application Plugin (useful if you are working on an executeable application)
    - Automatically applies the base Java plugin
    - Adds additional build config for speicifying the main class.
    - Adds additional tasks to run the executeable application.

## Gradle Tasks 
Tasks are the basic unit of work in Gradle.
Task belong to projects or subprojects and each can have different tasks based on the plugins applied.
```
 ./gradlew tasks //displays subset of tasks  
 ./gradlew tasks --all  // displays all tasks available 
 ./gradlew :subproject:taskname // how to specify task on a app subproject
```

In order for your task to display dependencies you ned to set the following config on gradle.properties
```
org.gradle.console=verbose
```

When a task is run on the root project it might also run the same task on subprojects.

Output files are typically put in the build directory. Outputs of the tasks file should be excluded from checking into version control.

## Gradle Properties
Gradle properties can be configured in multiple locations. 
1. Command line, set using the -D flag
2. gradle.properties file in the GRADLE_USER_HOME directory.
3. gradle.properties file in the projects directory, then its parents project directory up to the build's root directory.
4. gradle.properties file in the Gradle installation directory.

### -