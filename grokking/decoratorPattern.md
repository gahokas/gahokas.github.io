---
layout: default
---
# Grokking the Java Interview (page 92)

# What is the Decorator design pattern?


With the decorator design pattern, an object's functionality is extended (decorated) at dynamically at run-time. Decoration can happen on a per-instance basis, so not all objects of the same class have to be decorated. Furthermore, an object can be decorated without that object having any knowledge or concern about what additional functionality has been added, and without adding logic control statements (if / then / else).

There are some examples at ()[https://en.wikipedia.org/wiki/Decorator_pattern] that we base our explanation on below on.

## Example

Let's say we have a User object that we want to enhance (decorate). The User class has a method performAction(), and when a user performs an action, we want to add functionality to 1) log the user's actions to a logging mechanism, and 2) call an external API.

We start with a User interface, and a concrete base class that implements the interface:
```java
public interface User {

   void performAction(String actionName);

}
```

```java
public class SimpleUser implements User {

   @Override
   public void performAction(String actionName) {
      //perform some action
   }
}
```

We then add an abstract decorator class that our logging and API decorators will extend:

```java
public abstract class UserDecorator implements User {

   private final User userToBeDecorated;

   public UserDecorator(User userToBeDecorated) {
      this.userToBeDecorated = userToBeDecorated;
   }

   @Override
   public void performAction(String actionName) {
      userToBeDecorated.performAction(actionName);
   }

}
```
The subtle but important parts here are that the base decorator class is 1) abstract, so decorators adding specific functionality can extend it, and 2) it keeps a copy of the User it decorates, so decorated SimpleUsers will still perform their actions.

Next, each of our decorators extend our base decorator, and implement their specific functionality:

```java
public class UserLoggingDecorator extends UserDecorator {

    public UserLoggingDecorator(User userToBeDecorated) {
        super(userToBeDecorated);
    }

    @Override
    public void performAction(String actionName) {
        super.performAction(actionName);
        logSomeStuff();
    }

    private void logSomeStuff() {
        System.out.println("UserLoggingDecorator is doing something");
    }
    
}

public class UserAPICallDecorator extends UserDecorator {

    public UserAPICallDecorator(User userToBeDecorated) {
        super(userToBeDecorated);
    }

    @Override
    public void performAction(String actionName) {
        super.performAction(actionName);
        callSomeAPI();
    }

    private void callSomeAPI() {
        System.out.println("UserAPICallDecorator is doing something");
    }
    
}
```

When a decorator performAction() method is called, it will perform its super class' (UserDecorator) performAction, which calls our base User's performAction(). After that the specific decorated actions get called (logSomeStuff() or callSomeAPI()).

We can use our decorators as
```java
public class App {
    public static void main(String[] args) throws Exception {
        System.out.println("Hello, World!");
      
        User user = new SimpleUser();
        user.performAction("action 1");
        System.out.println("----------------");

        user = new UserLoggingDecorator(user);
        user.performAction("action 2");
        System.out.println("----------------");

        user = new UserAPICallDecorator(user);
        user.performAction("action 3");

    }

}
``` 
We simply pass in our User object to the decorator. Since the decorators implements the User interface, we can use the same User variable (if we want to) and the User has no knowledge at all about what extra decorated functionality has been added, nor how it is implemented.

The output of the above would be:
```
Hello, World!
SimpleUser performing action action 1
----------------
SimpleUser performing action action 2
UserLoggingDecorator is doing something
----------------
SimpleUser performing action action 3
UserLoggingDecorator is doing something
UserAPICallDecorator is doing something
```
[Sample code here](https://github.com/gahokas/grokking)
