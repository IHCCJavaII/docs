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
  - Inside the `VM options` textbox add the following line, making sure to update your JavaFX path to the one on YOUR computer:
  - `--module-path "/Users/jameswarner/javafx-sdk-23.0.2/lib" --add-modules javafx.controls,javafx.fxml`
4. Import any needed libraries and plugins in your pom.xml. Different projects may require different imports through maven, and you'll import them as dependencies like normal here.

