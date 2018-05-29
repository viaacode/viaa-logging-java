# viaa-logging-java

Java-implementation of VIAA's JSON layout logstash logger. It uses Log4j 2.x to send json-formatted logrecords to logstash over an UDP-socket.

## Synopsis

TODO

## Installation

**Repository** to add in your Maven-based project's ```pom.xml```.

```xml
<repositories>
  ...
  <!-- VIAA repositories -->
  <repository>
    <id>be.viaa</id>
    <name>VIAA Repository</name>
    <url>http://do-qas-mvn-01.do.viaa.be:8081/nexus/content/repositories/releases/</url>
    <layout>default</layout>
  </repository>
  ...
</repositories>
```

**Dependency** to add in your Maven-based project's ```pom.xml```. Alternatively, you can add the repository in the server- and repositories-sections of your Maven's ```settings.xml```. (See: [https://maven.apache.org/settings.html](https://maven.apache.org/settings.html)).

```xml
<dependencies>
  ...
  <!-- VIAA dependencies -->
  <dependency>
    <groupId>be.viaa</groupId>
    <artifactId>viaa-logging-java</artifactId>
    <version>1.0.0-SNAPSHOT</version>
  </dependency>
  ...
</dependencies>
```

This will download and install the SNAPSHOT-version into your project.

Build your project:

```$ mvn package```

Or, in Mule: right-click on project --> Mule --> Update Project Dependencies.

## Usage

Sample ```log4j2.xml``` file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Configuration packages="be.viaa.log4j2">

    <Appenders>
        <RollingFile name="file" fileName="/path/to/logs/your-app-name.log" 
                 filePattern="/path/to/logs/your-app-name.log.%i">
            <PatternLayout pattern="%d [%t] %-5p %c - %m%n" />
            <SizeBasedTriggeringPolicy size="10 MB" />
            <DefaultRolloverStrategy max="10"/>
        </RollingFile>
        <Socket name="logstash" host="logstash-host" port="logstash-port" protocol="UDP">
            <CustomJSONLayout />
        </Socket>
        <Rewrite name="rewrite">
            <AppenderRef ref="logstash"/>
            <PropertiesRewritePolicy>
                <Property name="APPLICATION">your-app-name</Property>
            </PropertiesRewritePolicy>
        </Rewrite>
    </Appenders>

    <Loggers>
        <AsyncRoot level="INFO">
            <AppenderRef ref="file" />
            <AppenderRef ref="rewrite" />
        </AsyncRoot>
    </Loggers>
</Configuration>
```

**Note the ```packages="be.viaa.log4j2"``` attribute on the Configuration element!**

