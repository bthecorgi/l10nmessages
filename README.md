# L10nMessages

[![Build](https://github.com/pinterest/l10nmessages/actions/workflows/build.yml/badge.svg)](https://github.com/pinterest/l10nmessages/actions/workflows/build.yml)
[![License](http://img.shields.io/:license-Apache%202-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.txt)
[![Maven Central](https://img.shields.io/maven-central/v/com.pinterest.l10nmessages/l10nmessages)](https://pinterest.github.io/l10nmessages/docs/installation)
[![javadoc](https://javadoc.io/badge2/com.pinterest.l10nmessages/l10nmessages/javadoc.svg)](https://javadoc.io/doc/com.pinterest.l10nmessages/l10nmessages)

`L10nMessages` is a library that makes internationalization (i18n) and localization (l10n) of Java
applications easy and safe.

It provides a [fluent API](https://pinterest.github.io/l10nmessages/docs/fluent-api) to load and 
format messages that is built on top of Java standard libraries. In addition to simplifying the 
overall setup, it brings strong typing and compile time checks to the mix when used with the 
[annotation processor](https://pinterest.github.io/l10nmessages/docs/annotation-processor).

## Full Documentation

The full documentation is [here](https://pinterest.github.io/l10nmessages/).

## Installation

See [general instructions](https://pinterest.github.io/l10nmessages/docs/installation) and 
guides for [Bazel](https://pinterest.github.io/l10nmessages/docs/installation/bazel),
[Gradle](https://pinterest.github.io/l10nmessages/docs/installation/gradle) and
[Maven](https://pinterest.github.io/l10nmessages/docs/installation/maven).

## Getting Started

Also available in the
[full documentation](https://pinterest.github.io/l10nmessages/docs/getting-started/) (with file
paths).

### Create a resource bundle

Create a root file: `Messages.properties` in the `com.pinterest.l10nmessages.example` package.

The corresponding resource bundle `baseName` is `com.pinterest.l10nmessages.example.Messages`.

`UTF-8` is the recommended encoding for the `properties` files.

In a project that follows the Maven layout, the file would be:

```properties title="src/resources/java/com/pinterest/l10nmessages/example/Messages.properties"
welcome_user=Welcome {username}!
```

### Register the resource bundle with `@L10nProperties`

Add the `@L10nProperties` annotation to the application class to register the resource bundle with
the annotation processor.

```java title="src/main/java/com/pinterest/l10nmessages/example/Application.java"
import com.pinterest.l10nmessages.L10nProperties;

@L10nProperties(baseName = "com.pinterest.l10nmessages.example.Messages")
public class Application {

}
```

## Enum generated by the annotation processor

Compile your project. The annotation processor should generate the following `enum`:

```java title="target/generated-sources/annotations/com/pinterest/l10nmessages/example/Messages.java"
package com.pinterest.l10nmessages.example;

public enum Messages {
  welcome_user("welcome_user");

  public static final String BASENAME = "com.pinterest.l10nmessages.example.Messages";
  // ...
}
```

### Strong typing using Enum

That `enum` can be used to create the `L10nMessages` instance and then to format a message using the
typed key: `Messages.welcome_user`. The argument: `username` is provided as a key/value pair.

```java title="src/main/java/com/pinterest/l10nmessages/example/Application.java"
import com.pinterest.l10nmessages.L10nMessages;
import com.pinterest.l10nmessages.L10nProperties;

@L10nProperties(baseName = "com.pinterest.l10nmessages.example.Messages")
public class Application {

  public static void main(String[] args) {
    L10nMessages<Messages> m = L10nMessages.builder(Messages.class).build();
    String localizedMsg = m.format(Messages.welcome_user, "username", "Mary");
    System.out.println(localizedMsg);
    // Welcome Mary!
  }
}
```

For extra typing, consider [argument names typing](https://pinterest.github.io/l10nmessages/docs/fluent-api#argument-names-typing).

### Localization

Localize the root properties file by creating the file `Messages_fr.properties` for "French"

```properties
welcome_user=Bienvenue {username}!
```

Specify the locale wanted for the messages

```java

@L10nProperties(baseName = "com.pinterest.l10nmessages.example.Messages")
public class Application {

  public static void main(String[] args) {
    L10nMessages<Messages> m = L10nMessages.builder(Messages.class)
        .locale(Locale.FRENCH)
        .build();
    String localizedMsg = m.format(Messages.welcome_user, "username", "Mary");
    System.out.prinln(localizedMsg);
    // Bienvenue Mary!
  }
}
```
