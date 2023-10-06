# Table of contents
1. [How to use With a Maven plugin](#with-a-Maven-plugin)
2. [How to use With a standalone liquibase binary](#with-a-standalone-liquibase-binary)
3. [How to test locally](#how-to-test-locally)

# How to use
There are two distinct ways liquibase-impala can be used to manage your Impala or Hive database.

## With a Maven plugin
To use liquibase-impala in concert with ```liquibase-maven-plugin```:
1. Make sure liquibase-impala is present in your local or remote (internal) Maven repo.
2. Add the following to your ```pom.xml``` file:
```xml
<build>
  <plugins>
    <!-- (...) -->
    <plugin>
      <groupId>org.liquibase</groupId>
      <artifactId>liquibase-maven-plugin</artifactId>
      <version>${liquibase.version}</version>
      <dependencies>
        <!-- (...) -->
        <dependency>
          <groupId>org.liquibase.ext.impala</groupId>
          <artifactId>liquibase-impala</artifactId>
          <version>${liquibase.impala.version}</version>
        </dependency>
      </dependencies>
    </plugin>
  </plugins>
</build>
```

3. Run Liquibase as you normally would using Maven plugin, for example:
```
mvn liquibase:update \
  -Dliquibase.changeLogFile=changelog/changelog.xml \
  -Dliquibase.driver=com.cloudera.hive.jdbc.HS2Driver \
  -Dliquibase.username=<user>
  -Dliquibase.password=<password>
  -Dliquibase.url=jdbc:hive2://<host>:<port>/<database>;UID=<user>;UseNativeQuery=1
```

## With a standalone liquibase binary
1. Make sure that ```liquibase``` is on your ```$PATH```
2. Modify ```liquibase.properties``` according to your Impala/Hive endpoint
3. Put liquibase-impala fat-jar on your classpath, f.e. under the ```${LIQUIBASE_HOME}/lib```
4. Start migration, f.e.: ```liquibase update```

## How to test locally
Script ```examples/run.sh``` performs basic integration testing of Impala and Hive, which includes:
* update execution
* tag execution
* rollback execution

The script can be executed with the command ```./run.sh <both|hive|impala> PATH_TO_LIQUIBASE_HOME```