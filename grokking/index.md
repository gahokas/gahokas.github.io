---
layout: default
---
# Grokking the Java Interview - Book Review

[Javin Paul's](https://twitter.com/javinpaul){:target="_blank"} book [Grokking the Java Interview](https://gumroad.com/l/QqjGH){:target="_blank"} is a broad range of Java-based topics intended to assist Java developers prepping for the interview process.

### What's in the book
The book contains 159 pages of Java questions that a Java developer would (ideally) be able to answer. Topics include:
 * Object Oriented programming (abstraction, polymorphism, overloading, overriding)
 * Core Java (String manipulation, basic algorithms, sorting, equality)
 * Multi-threading (race conditions, thread communication and sychronization techniques)
 * Collections (Sets, Maps, Lists)
 * Serialization (What is it, how to do it)
 * Design Patterns (Singletons, Factories, Decorators)
 * Garbage Collection (Types of garbage collection, monitoring, memory management)
 * Generics (What are they, using them in classes and collections, wildcards)
 * JDBC (What is it, locking, pooling)
 * Functional Programming (Streams, map, filter)

Following each question there is a short paragraph explanation or answer.

### Who is this book for
You are
 * an intermediate to senior Java developer, looking to brush up some skills before interviewing, and want a checklist of core Java topics to cover
 * a developer or manager looking for core Java interview question ideas

### Who is it not for
You are
 * just learning programming (or Java). If you are new to programming or Java, just be cautious and come in with realistic expectations. The topic list in the book is in-depth and _very_ extensive, and it could be intimidating or even discouraging to see 159 pages of words and topics you don't know.

### Opinion
I am a senior Java developer with 12+ years of Java-specific experience, and the first time through the book, I didn't know, or didn't have at least some idea on, about half of the answers. The breadth of topics covered is just so vast (as is Java itself) and I don't have to use (regularly) many aspects of the language in my day to day work (ex concurrency, some design patterns, and garbage collection specifics). I could easily spend an hour a night for a month or two going through the questions and investigating the answers. Some questions are quick (Can a class extend more than one class?), some can take many hours of learning to work out a solution (How do you solve to the Producer / Consumer problem).

I found that sometimes the answers to the questions aren't complete, but rather guidance on the approach required to solve the problem. This means you can't just read the book front to back and "know" the answers to the questions. Many questions require the reader to open up a code editor to test, experiment, and learn the answer for themselves. This is the only way to truly "learn" and internalize the answers anyway. Just memorizing the answers without understanding does you no good.

### Bottom line
I think for a developer just learning Java, the book might be a little intimidating. But if you have some Java experience, and want to use the book as a checklist to ensure you're aware of the majority of topics related to core Java prior to a technical interview, this book is an excellent tool for that.

### Sample Questions (and my answers)
Here are some sample questions I found tricky (or just didn't know or remember) along with some code I wrote as I investigated the answers.

* [Page 34: Can we have a return statement in a finally block?](finallyInReturn)

* [Page 51: Write a program to solve the Producer Consumer problem in Java](producerConsumer)

* [Page 86: How do you serialize an object in Java?](serialization)

* [Page 90: What is the Decorator design pattern?](decoratorPattern)
