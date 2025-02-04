---
title: Resource Bundle
---

`L10nMessages` uses `java.util.ResourceBundle` to load localized messages and specifically focuses
on improving the experience when working with resource bundles that are backed by `properties`
files.

In plain Java, there is are no safeguard to ensure `properties` files are present, nor that the
messages are present and valid which can lead to hard failure at runtime.

The [Annotation Processor](annotation-processor) can be used to validate the existence of the
`properties` files at build time and to check the messages' validity. It will also generate an
`enum` that can be then used to initialize the fluent API and to provide strong typing of message
keys insuring the existence of messages at runtime.

The [Fluent API](fluent-api) can simply be initialized from the `enum` and will seamlessly load
messages from the resource bundle. It can also be used without the annotation processor, in which
case the resource bundle `baseName` needs to be provided manually and message key will be untyped
strings.

## Naming convention

Java uses the notion of `baseName` to identify a `ResourceBundle`.

The `baseName` follows the pattern `{package}.{bundleName}` where `package` is a regular Java
package in "dotted" notation. The `package` and `bundleName` are defined by the user.

The root properties filename follows the pattern: `{bundleName}.properties` and the localized
properties filename follows the pattern: `{bundleName}_{locale}.properties`.

For example, with the bundle name: `Messages` and package: `com.pinterest.l10nmessages.example`, the
base name will be: `com.pinterest.l10nmessages.example.Messages`, the root properties file will be:
`Messages.properties` and the localized properties for French will be `Messages_fr.properties`.

With `Maven` directory layout, the files will be:

```properties title="src/resources/java/com/pinterest/l10nmessages/example/Messages.properties"
welcome=Welcome!
```

```properties title="src/resources/java/com/pinterest/l10nmessages/example/Messages_fr.properties"
welcome=Bienvenue!
```

## Content and encoding

`properties` files are simple plain text files. They contain a list of messages that are identified
by keys. Use a comment before the key to provide translator instruction.

```properties
# Translator instruction for message1
key1=message1

# Translator instruction for message2
key2=message2
```

`UTF-8` is recommended for the file content. For legacy purpose, `ISO-8859-1` is also supported.

With Java 8, the library uses a similar logic as what became standard with Java 9. It tries to load
the `properties` as `UTF-8` and fallbacks to `ISO-8859-1` in case of errors. With Java 9+, the JDK
code is directly used.

## Other ResourceBundle types

The fluent API can use any customized `ResourceBundle`. The annotation processor is only useful for
the ones that are backed by properties files.
