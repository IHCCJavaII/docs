# Testing System.out.println in JUnit 5 Tests

In console applications, it may be handy to assert the result of a print statement. While much of your core logic can be tested without relying on this, sometimes you will need to test what the user sees. In the case of a console application that will be what is printed to the user.

Remember that JUnit tests run in their own environment. We can take advantage of this by controlling what `System` holds during the JUnit session. 

`System.out` is really a `ByteArrayOutputStream` object that the console writes to when a user types characters and hits enter. The problem is that by default, we don't have access to check what was written to that stream. Fortunately, `System.setOut` is a method available to set the outputstream that `System` uses to our own.

By creating our own `ByteArrayOutputStream` and setting `System.out` to hold our stream, we can have direct access to check what was loaded into that stream for the entirety of the JUnit session.

## Step 1: JUnit Setup

Make sure you have added JUnit as a dependency by following the steps here:
[JUnit Setup](junit-setup-instructions.md)

## Step 2: Set up objects

Create two final instance variables inside the JUnit test class. The `ByteArrayOutputStream` will be passed to `System.out`, and the `PrintStream` will be used to later to reset `System.out` to it's default `PrintStream` (to avoid interference for any tests that we do NOT want to intercept print statements).

```
private final PrintStream defaultOut = System.out;
private final ByteArrayOutputStream testOutputStream = new ByteArrayOutputStream();
```

## Step 3: Set up BeforeEach and AfterEach methods

JUnit has nifty annotations you can use to automatically call a method of your choosing before every test. There is also an annotation to call a method after each test.

This method will set the `System.out` for the JUnit session equal to our outputStream we created earlier. This is the key to being able to capture and see what gets written to the outputStream whenever a `System.out.println` runs.

```
@BeforeEach
public void setOutputStream() {
    System.setOut(new PrintStream(testOutputStream));
}
```

This part is only necessary to reduce interference with other tests, especially any tests where we DONT care to intecept the print results. It runs after each test, and resets `System.out` back to the default, which we captured earlier in our instance variables.

```
@AfterEach
public void resetSystemOut() {
    System.setOut(defaultOut);
}
```

Remember that both `@BeforeEach` and `@AfterEach` annotated methods will run at the appropriate times without having to be explicitly called. 

## Step 3: Use in a test

Now that the set up is done, you can assert for what was displayed in a `System.out.println` statement by checking our copy of the `testOutputStream`. It will be filled with whatever strings were written to `System.out` when the test code was executed.

```
@Test
public void whenSomething_thenSomething() {
    // ..... do test setup, perform an action ......
    assertEquals("Expected string outputted to user", testOutputStream.toString().trim());
}
```

Note you can play around with the trim. It is to remove the newline character that appears when `println` is used versus `print`. You can also hardcode `\n` inside your expected string to capture this or any new lines that appear inside your actual string.