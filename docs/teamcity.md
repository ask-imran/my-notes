 ---
layout: default
title: Teamcity and Kotlin
nav_order: 4
---

 # Teamcity and Kotlin

 ### Scramble

 - Kotlin is converted into xml for teamcity

    - `/.m2/` path to local repository on machine
    - `/target` a folder where executable file (compiled classes, test reports etc) is stored

### Demystifying POM.xml
 - `pom.xml` (Project Object Model) implies that .teamcity is a maven project, it contains all configuratins about project assembly
    - Maven is a **declrative build automation tool** (like ant, gradle), automates code compilation, test run, dependency import etc

```xml
<?xml version="1.0"?>
<project>
  <modelVersion>4.0.0</modelVersion>
  <name>Anewtodolist Config DSL Script</name>
  <groupId>Anewtodolist</groupId>
  <artifactId>Anewtodolist_dsl</artifactId>
  <version>1.0-SNAPSHOT</version>

  <properties>
    <kotlin.version>1.8.22</kotlin.version>
  </properties>

  <parent>
    <groupId>org.jetbrains.teamcity</groupId>
    <artifactId>configs-dsl-kotlin-parent</artifactId>
    <version>1.0-SNAPSHOT</version>
  </parent>

  <repositories>
    <repository>
      <id>jetbrains-all</id>
      <url>https://download.jetbrains.com/teamcity-repository</url>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
    </repository>
  </repositories>

  <dependencies>
    <dependency>
      <groupId>org.jetbrains.teamcity</groupId>
      <artifactId>configs-dsl-kotlin-latest</artifactId>
      <version>${teamcity.dsl.version}</version>
      <scope>compile</scope>
  </dependency>

  <build>

  </build>
```

- `pom.xml` starts with `xml` tag followed by the `project` tag 
- `<modelVersion>` specifies the version of POM the file adheres to. Ensures compatability between different versions of Maven schema
- `<groupId>` unique identifier for the org or group that owns the project (ex. `org.jetbrains.teamcity` similar to scope in node)
- `<artifactId>` unique identifier for the project itself within the group (similar to the name of the package in node)
- `<version>` version of the project, `-SNAPHOT` means active development and frequent version changes and maven ensures to have some sort of unique identifier (like timestamp or something)
- `<properties>` similar to varaibles, serves as placeholders for values that can be reused throughout the POM file ex. `${kotlin.version}`
- `<parent>` specifies the parent project of the curent project, allowing the current project to inherit certain configurations, dependencies and plugins from the parent project. Helps with maintaining consistency and reduces duplication of configuration
    * a maven project can only have one parent project, however, it can still be acheieved by using project aggregation (aka multi module projects) 
- `<dependencies>` This is where dependency required by the project is declared each `<depdendency>` within a `<dependencies>` specify a single dependency (including the artifactId, groupId and version)
    * maven uses this information to download required libs/frameworks
- `<repositories>` define remote repositories where maven should search for project `dependencies` that are not available in the local respository
- `<pluginRepositories>`similar to respositories the `pluginRepositories` define remote repositories specifically for maven plugins

### Demystifying Teamcity

 - Basic `settings.kts` file for build configuration contains.
    1. version (ex. "2020.1") which referes to the teamcity server version
    2. `project` block defines the project
    3. `project` calls `buildType` referencing a singleton object
    4. `buildType` (in Kotlin) is the same as `buildConfiguration` (in UI)
    5. A `buildType/buildconfiguration` can have multiple build steps
    6. `buildFeature`
    7. `buildChains` `sequential` and `dependencies`
    8. `class` vs `object` vs `data class` vs `val` and `this`
    9. `contextParameters`

### Kotlin
-  `var` and `val`
- Collection types
  - List - Store items in order that they are added and accessed via indexed access operator
      - `listOf()` or `mutableListOf()`
  - Sets - Unordered & only store unique items and **can't access an item at a particular index**
      - `setOf()` or `mutableSetOf()`
  - Maps - Store items as key-value pairs, access the item by referencing the key
      - `mapOf` or `mutableMapOf()`
- Control flow
  - `if` - No ternary operator in Kotlin, instead `if` can be used as an expression ex. `println(if(a > b) a else b)`
  - `when` similar to `switch` *(not loop)*. `->` is used to separate each condition from each action
  - Ranges (`..`) - Construct ranges for loops to iterate
    - `1..4` - 1,2,3,4 (same of alphabets)
    - `1..<4` - 1,2,3
    - `4 downTo 1` - 4,3,2,1
    - `1..5 step 2` - 1,3,4 i.e increment that isn't 1
  - Lambda Function - written within the `{}` block ex. `{string:String -> string.toUpper()}`
  - Classes
    - class header content within the `()` ex. `class Contact(val id:int)`
    - constructors are automatically created with parameters defined, instance of a class doesn't require `new` keyword, just call the class
    - `member functions`

    ### Resources
    * [Kotlin Playground](https://play.kotlinlang.org/byExample/01_introduction/01_Hello%20world)