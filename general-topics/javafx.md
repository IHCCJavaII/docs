# Getting Started with JavaFX

JavaFX is a graphical library for Java that will allow you to create buttons, forms, tables, and other graphics for your Java program. You can even style these elements with CSS! 

[Read this basic tutorial for how JavaFX works](https://dev.java/learn/javafx/)

## Setting up JavaFX for the first time

JavaFX used to be included in the normal JDK, but that hasn't been the case for years now. To use JavaFX you need to install the JavaFX library separately, and point IntelliJ (or whatever launcher you're using) to where to find that library of code).

### Steps to setup JavaFX
1. Download the latest version of JavaFX for your system (make sure to match your OS): [Download JavaFX](https://gluonhq.com/products/javafx/)
2. Extract the download into a location on your computer that is simple and you'll remember. For example, directly on your C drive or under your user directory. Copy that path and use it in the next steps.
3. In IntelliJ, for each project you use JavaFX in, you need to update the `run configurations` to include pointing to that JavaFX library on your computer. To do this:
  - With an IntelliJ project open, navigate to: `Run -> Edit Configurations -> Add application -> Modify Options -> Add VM options`
  - Name your configuration `JavaFX` so that you can remember it easily later
  - Inside the `VM options` textbox add the following line, making sure to update your JavaFX path to the one on YOUR computer:
  - `--module-path "/Users/jameswarner/javafx-sdk-23.0.2/lib" --add-modules javafx.controls,javafx.fxml`
  - In the section requiring you to select a main class, click on the icon to the right of the textbox to select your main class from the dropdown. For example, if this was a brand new default project the correct class would be `org.example.Main` where `org.example` is the default package name and `Main` is the default main class. If you rename your `Main` class or update your package structure, you'll need to update this.
  - You can save the configuration as a file so that you can copy it into future projects and don't have to setup this configuration every time you make a new JavaFX project. [Read how to save configurations here](https://www.jetbrains.com/help/idea/run-debug-configuration.html#share-configurations)
  - If you're running into any issues, see the screenshots and [IntelliJ's official docs for Run Configurations here](https://www.jetbrains.com/help/idea/run-debug-configuration-java-application.html)
4. Import any needed libraries and plugins in your pom.xml. Different projects may require different imports through maven, and you'll import them as dependencies like normal here. See the next section below.

### Adding Maven Dependenices

In your `pom.xml` you will need to include any dependencies you're relying on from `JavaFX`. Instead of these packages coming from an external Maven repo like we've done in the past with `JUnit`, your program will look for these on your computer where you saved the `JavaFX SDK`. That's why you did what you did to setup the run configurations. You're telling the program where to look for these dependencies. 

Depending on your project, you may use different features in `JavaFX`. Here's a basic grouping of dependencies that will get you started:

```xml
<dependencies>
        <dependency>
            <groupId>org.openjfx</groupId>
            <artifactId>javafx-controls</artifactId>
            <version>23</version>
        </dependency>
        <dependency>
            <groupId>org.openjfx</groupId>
            <artifactId>javafx-fxml</artifactId>
            <version>23</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>23</source>
                    <target>23</target>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.openjfx</groupId>
                <artifactId>javafx-maven-plugin</artifactId>
                <version>0.0.8</version>
                <configuration>
                    <mainClass>TheNameOfYouMainClassHERE</mainClass> <!-- CHANGE THIS TO YOUR CLASS NAME THAT HAS THE MAIN METHOD IN IT -->
                </configuration>
                <executions>
                    <execution>
                        <id>default-cli</id>
                        <goals>
                            <goal>run</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```


