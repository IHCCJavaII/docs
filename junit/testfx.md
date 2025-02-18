# Using JUnit to test JavaFX project using TextFX

Normal JUnit tests will not work to test methods that rely on JavaFX components. This is because JavaFX is a special environment, and unless that environment starts up correctly those JavaFX components will not be initialized.

But we still need to test JavaFX projects just as much as other projects - what can we do?!

Thankfully, developers have solved this by creating a library of code that adds on to JUnit and allows you to properly unit test JavaFX projects. It also adds cool features for testing the UI components. It's called `TextFX`.

## Setting up TextFX

Two depedencies need to be added to your `pom.xml` file. The following versions are the latest as of writing this, but you should check for the latest versions if you have any issues.

```xml
    <!-- https://mvnrepository.com/artifact/org.testfx/testfx-core -->
    <dependency>
        <groupId>org.testfx</groupId>
        <artifactId>testfx-core</artifactId>
        <version>4.0.18</version>
        <scope>test</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.testfx/testfx-junit5 -->
    <dependency>
        <groupId>org.testfx</groupId>
        <artifactId>testfx-junit5</artifactId>
        <version>4.0.18</version>
        <scope>test</scope>
    </dependency>
```

## Writing your first test with JUnit and TestFX

There are two main pieces using `TextFX` for getting your `JUnit` tests to work with `JavaFX`.

Change your test class to extend `Application`, just like you do in your main application.

```java
public class RetailDataAnalyticsTests extends ApplicationTest {
   ....
}
```

Override the `start` method from `Application` so that the `JavaFX` environment starts before your unit tests run.

```java
    // declared at the test class level, so that it can be accessed in each test
    private RetailDataAnalytics retailDataAnalytics;

    @Override
    public void start(Stage stage) {
        retailDataAnalytics = new RetailDataAnalytics();
        retailDataAnalytics.start(stage);
    }
```

Once you have this setup, you can run your unit tests as normal!

## Testing JavaFX UI components with TestFX

You can use built-in `TestFX` methods to examine and interact with `JavaFX` graphical elements! Here are some examples:

```java
    // Finds a button on the screen and verifies the text and that the button is clickable.
    @Test
    public void testUploadButtonExistsAndClickable() {
        verifyThat(".button", hasText("Upload CSV File"));
        clickOn(".button");
    }
```

```java
    // Finds the text field on the screen that is being used for filtering. Writes "shoes" as the filter option.
    // asserts that the table data has updated after the filter has ran.
    @Test
    public void testFilterProducts() {
        verifyThat(".text-field", hasText(""));

        clickOn(".text-field").write("Shoes");

        TableView<String[]> table = retailDataAnalytics.tableView;
        assertEquals(1, table.getItems().size());
    }
```

Read more about TestFX and the available methods in the [documentation here](https://testfx.github.io/TestFX/)
