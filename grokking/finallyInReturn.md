---
layout: default
---
# Telephonic round 19) Can we have return statement in finally clause? What will happen?

This is an interesting question. I don't think I've ever attempted to put a return statement inside a finally block in my professional coding career. Typically finally blocks are used as clean up code from previous try blocks, and I would put any return statement at the end of the method.  However there are a couple of interesting learnings from attempting it. Suppose we have the following code:

```java
public class App {
    public static void main(String[] args) throws Exception {
        System.out.println("Hello, World!");
        String result = calculate();
        System.out.println("Result: " + result);      
    }

    public static String calculate() {
        try {
            System.out.println("Calculating...");
            int result = 1 + 2 + 3;
            return "Answer: " + result;            
        }
        finally {
            System.out.println("In finally");
            return "Return from finally";
        }
        
    }
}
```

**What will happen?**

Well, return statements inside finally blocks are permitted. You will get a warning
> finally block does not complete normally

because of the return statement, but it will compile and run. As with other return statements, you can't have any code after it, either in the finally block or at the end of the method. You will get a compile error:
> unreachable code

But what about the return statement in the try block vs the finally block? The return statement in the finally block will _override_ the return statement in the try block, so the code will output
```
Hello, World!
Calculating...
In finally
Result: Return from finally
```

Interestingly, the return statement in the try block is not executed! Generally I would advise against this coding practice, as I think this would lead to non-obvious code and make it more difficult to understand and maintain for other developers on the team, but it is handy to know in case you encounter it.
