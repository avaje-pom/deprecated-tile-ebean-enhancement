# ebean-enhancement tile

Maven tile that performs the Ebean enhancement and Query bean enhancement.

## Example use

In your project pom under build / plugins add the tiles-maven-plugin with the following configuration. Note that this example also brings in the java-compile tile.

```xml
      <!-- maven build / plugins -->

      <plugin>
        <groupId>io.repaint.maven</groupId>
        <artifactId>tiles-maven-plugin</artifactId>
        <version>2.8</version>
        <extensions>true</extensions>
        <configuration>
          <tiles>
            <tile>org.avaje.tile:ebean-enhancement:1.4</tile>
          </tiles>
        </configuration>
      </plugin>

```

This second example also brings in the java-compile tile to configure the *maven-compiler-plugin*. 

```xml
      <!-- maven build / plugins -->

      <plugin>
        <groupId>io.repaint.maven</groupId>
        <artifactId>tiles-maven-plugin</artifactId>
        <version>2.8</version>
        <extensions>true</extensions>
        <configuration>
          <tiles>
            <tile>org.avaje.tile:java-compile:1.1</tile>
            <tile>org.avaje.tile:ebean-enhancement:1.4</tile>
          </tiles>
        </configuration>
      </plugin>

```


## What it does

Effectively the kotlin-compile tile brings in the *kotlin-maven-plugin* with configuration for compiling *main* and *test* code.

```xml
  <!-- defaults, override in your project pom if needed -->

  <properties>
    <ebeanorm-enhancement.plugin.args>debug=0</ebeanorm-enhancement.plugin.args>
    <querybean-maven-plugin.version>2.2.1</querybean-maven-plugin.version>
    <avaje-ebeanorm-mavenenhancer.version>4.11.1</avaje-ebeanorm-mavenenhancer.version>
  </properties>

  <!-- brought into build / plugins -->

  <build>
    <plugins>

      <plugin>
        <groupId>org.avaje.ebeanorm</groupId>
        <artifactId>codegen-maven-plugin</artifactId>
        <version>${codegen-maven-plugin.version}</version>
      </plugin>

      <plugin>
        <groupId>org.avaje.ebeanorm</groupId>
        <artifactId>querybean-maven-plugin</artifactId>
        <version>${querybean-maven-plugin.version}</version>
        <executions>
          <execution>
            <id>main</id>
            <phase>process-classes</phase>
            <configuration>
              <transformArgs>${ebeanorm-enhancement.plugin.args}</transformArgs>
            </configuration>
            <goals>
              <goal>enhance</goal>
            </goals>
          </execution>
          <execution>
            <id>test</id>
            <phase>process-test-classes</phase>
            <configuration>
              <classSource>target/test-classes</classSource>
              <transformArgs>${ebeanorm-enhancement.plugin.args}</transformArgs>
            </configuration>
            <goals>
              <goal>enhance</goal>
            </goals>
          </execution>
        </executions>
      </plugin>

      <plugin>
        <groupId>org.avaje.ebeanorm</groupId>
        <artifactId>avaje-ebeanorm-mavenenhancer</artifactId>
        <version>${avaje-ebeanorm-mavenenhancer.version}</version>
        <executions>
          <execution>
            <id>main</id>
            <phase>process-classes</phase>
            <configuration>
              <classSource>target/classes</classSource>
              <transformArgs>${ebeanorm-enhancement.plugin.args}</transformArgs>
            </configuration>
            <goals>
              <goal>enhance</goal>
            </goals>
          </execution>
        </executions>
      </plugin>

    </plugins>

  </build>

```
