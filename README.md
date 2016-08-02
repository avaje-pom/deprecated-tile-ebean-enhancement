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
            <tile>org.avaje.tile:ebean-enhancement:8.1</tile>
          </tiles>
        </configuration>
      </plugin>

```



## What it does

Effectively the ebean enhancement tile brings in 3 plugins:
- `avaje-ebeanorm-mavenenhancer` ... for enhancing Entity beans and @Transactional (in src/main and src/test)
- `querybean-maven-plugin` ... for enhancing "Query beans" (in src/main and src/test)
- `codegen-maven-plugin` ... for generating "Finders"

```xml
  <!-- defaults, override in your project pom if needed -->

  <properties>
    <ebeanorm-enhancement.plugin.args>debug=0</ebeanorm-enhancement.plugin.args>
    <querybean-maven-plugin.version>2.3.2</querybean-maven-plugin.version>
    <avaje-ebeanorm-mavenenhancer.version>8.1.1</avaje-ebeanorm-mavenenhancer.version>
    <codegen-maven-plugin.version>1.2</codegen-maven-plugin.version>
    <avaje-ebeanorm-mavenenhancer.target-test-classes>target/test-classes</avaje-ebeanorm-mavenenhancer.target-test-classes>
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
          <execution>
            <id>test</id>
            <phase>process-test-classes</phase>
            <configuration>
              <classSource>${avaje-ebeanorm-mavenenhancer.target-test-classes}</classSource>
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
