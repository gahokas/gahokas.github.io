---
layout: default
---
# Grokking the Java Interview (page 51)

# Write a program to solve the Producer Consumer problem in Java

The Producer consumer problem is a generalized problem that requires threads to safely coordinate access to a shared resource. The resource is not thread-safe, so only one thread may be modifying (adding or removing) from it at at time. This resource could be a shared data structure in memory, or a file on the filesystem, etc. 

## Detailed Requirements

We will use the shared data structure example. One or more Producer threads produce some output and add it to a shared queue (a first-in-first-out linked list) . A producer thread signals to a consumer thread when some output is produced. A consumer thread then removes the output from the shared queue consumes it.

For our purposes, we add some detailed requirements:
* A producer produces an item of output, adds it to the shared Queue, sleeps for 2 seconds, repeats this 5 times, and then quits.
* A producer signals to a consumer that a unit of output has been added to the shared queue, so the consumer "knows" when to check the queue. This prevents the consumer from having to "poll" the queue at regular intervals for output.
* A consumer waits for 10 seconds. If nothing has been added to the queue in 10 seconds, it assumes the producer is done and quits.

## General Approach

There are a few design approaches we could take with this problem:
* Have a thread-safe Queue, so that only one thread (Producer or Consumer) can access the Queue at a time (to either add or remove items)
* Have a non-thread-safe Queue, and require the Producer and Consumer to sychronize access to the Queue themselves.

The first approach nicely encapsulates all the complexities of the sychronization code within the Queue itself, leaving a simple implementation for the Producer and Consumer. It also encourages code re-use, as a thread-safe Queue could be useful for other projects, and used safely and easily by other programmers, even if they're not totally familiar with details of thread-safe implementations.

## Guarded Blocks

How do we make the Queue thread-safe? We need to guarantee that only one thread, a producer or consumer, is modifying (adding to or removing from) the Queue at at time? Java has the concept of a guarded block, a block of code or method that is guranteeded to be executed by only one thread at a time for a particular object. Within a guarded block, we can use Object.wait and Object.notify. The consumer uses Object.wait to wait for a notification from the producer that something has been added to the queue. The producer will use Object.notify to tell the consumer that something has been added.

It is important to realize that Object.wait and Object.notify MUST be used only when a thread owns the monitor of the shared Queue. From [Object.notify Java doc](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Object.html#notify()) we see that a thread becomes owner of an object's monitor:

* By executing a synchronized instance method of that object.
* By executing the body of a synchronized statement that synchronizes on the object.
* For objects of type Class, by executing a synchronized static method of that class.

Only one thread at a time can own an object's monitor. Our shared Queue can have sychronized instance methods get() and put(Object). When a producer or consumer thread is executing code within these sychronized methods, a producer or consumer thread will own the Queue's monitor, and therefore can use Object.notify() and Object.wait().

When a consumer calls Object.wait(), it releases its lock on the shared queue and suspends execution, and allows the producer to execute the put(Object) synchronized method to add something to the queue.

## Design

Our general design can be as follows:
* Our main method creates a shared Queue object
* The main method starts up Producer and Consumer threads, giving each a reference to the shared Queue
* The shared queue will use sychronized instance methods get() and put(Object) to ensure only one thread is accessing the Queue at a time

## Implementation

Our main App class:

```java
public class App {
    public static void main(String[] args) throws Exception {
        System.out.println("Hello, World!");
        SharedQueue sharedQueue = new SharedQueue();
        new Thread(new Producer(sharedQueue)).start();
        new Thread(new Consumer(sharedQueue)).start();
    }

}
```

Our Producer class produces 5 items of "output", and puts them on the queue.
```java
import java.util.Date;

public class Producer implements Runnable {

    private SharedQueue queue;

    public Producer(SharedQueue queue) {
        this.queue = queue;
    }
    
    @Override
    public void run() {

        for (int i = 0; i < 5; i++) {
            String output = new Date().toString();
            queue.put(output);
            log("produced something! Now taking a break");
            try {
                Thread.sleep(2000);
            }
            catch (InterruptedException e) {}
        }

        log("all done");
    }

    private void log(String message) {
        System.out.println("Producer " + Thread.currentThread().getName() + " " + message);
    }
    
}
```

Our consumer Class gets "output" from the queue, until the queue returns null, which we use to indicate the Producer is done and the Consumer can stop as well.

```java
public class Consumer implements Runnable {

    private SharedQueue queue;

    public Consumer(SharedQueue queue) {
        this.queue = queue;
    }

    @Override
    public void run() {

        while (true) {
            String output = queue.get();
            log("received " + output);
            if (output == null) {
                break;
            }
        }

        log("timed out waiting for producer.. I guess we're done");
    }

    private void log(String message) {
        System.out.println("Consumer " + Thread.currentThread().getName() + " " + message);
    }
    
}
```

For the SharedQueue implementation, the put method the Producer uses to add an item to the queue is straightforward:

```java
public class SharedQueue {

    private LinkedList<String> queue;

    public SharedQueue() {
        queue = new LinkedList<>();
    }

    public synchronized void put(String string) {
        queue.addLast(string);
        notify();
    }

    public synchronized String get() {
       //to be implemented
       return null;
    }

}
```

### Consumer Implementation

The SharedQueue uses a sychronized instance method get(), which calls notify() to tell the Consumer when it has added the output to the Queue. The consumer thread won't be able to proceed with taking an item off of the queue until the Producer finishes executing the get() method, since only one thread can be executing inside of a guarded block at a time per insance of the object (we only have one instance of the SharedQueue).


For the implementation of the SharedQueue.get() method, the requirements are:
* if the queue is not empty, return the first item off the queue
* otherwise, wait for up to 10 seconds for the Producer to notify that an item has been added to the queue
* if notified by the Producer, return the first item off of the queue
* If waited for 10 seconds without notification, return null (timeout)


The Consumer calls Object.wait() to wait for 10 seconds, from [Object.wait() JavaDoc](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Object.html#wait(long,int))

* Object.wait() will throw an InterruptedException when notified or interruped by another thread.
* Object.wait() will return normally when 10 seconds have expired
* Object.wait() will return normally before 10 seconds have expired, due to spurious wakeup

Exactly what spurious wakeups are and why they (rarely) occur is another topic, but we need to be aware of them. This adds complication to the implementation, because we need to check if Object.wait() is returning because it has timed out, or because a spurious wakeup has occurred. We need to keep track of how long the thread has slept, and when Object.wait() returns, if we have not slept long enough and there are still no objects on the Queue, we need to sleep more until 10 seconds has elapsed.

This leads to the complete implementation of our SharedQueue:

```java
import java.util.Date;
import java.util.LinkedList;

public class SharedQueue {

    private final int WAIT_TIME = 10000;
    private LinkedList<String> queue;

    public SharedQueue() {
        queue = new LinkedList<>();
    }

    public synchronized void put(String string) {
        queue.addLast(string);
        notify();
    }

    public synchronized String get() {
        int totalWaitTime = 0;
        while (queue.isEmpty() && totalWaitTime < WAIT_TIME) {
            try {
                long waitStartTime = new Date().getTime();
                long timeToWait = WAIT_TIME - totalWaitTime;
                wait(timeToWait);
                totalWaitTime += (new Date().getTime() - waitStartTime);
            }
            catch (InterruptedException e) {
            }
        }

        if (!queue.isEmpty()) {
            return queue.removeFirst();
        }
        
        return null;
    }
 
}
```

## Multiple Producers / Consumers

A feature of our implementation is that it is very easy to add more Producer and Consumer threads. The implementation could be used as the basis for a dynamic service that could automatically scale up / down by adding / removing threads, as demand changes.


```java
public class App {
    public static void main(String[] args) throws Exception {
        System.out.println("Hello, World!");
        SharedQueue sharedQueue = new SharedQueue();
        new Thread(new Producer(sharedQueue)).start();
        new Thread(new Producer(sharedQueue)).start();
        new Thread(new Producer(sharedQueue)).start();
        new Thread(new Consumer(sharedQueue)).start();
        new Thread(new Consumer(sharedQueue)).start();
        new Thread(new Consumer(sharedQueue)).start();
        new Thread(new Consumer(sharedQueue)).start();
    }

}
```

When multiple Producer / Consumer threads are running, each safely adds / removes from the first-in-first-out shared queue. Note that when SharedQueue.put() calls notify(), we don't actually know which Consumer thread will be notified (nor do we really care). It is up to the Java implementation to pick one.


[Sample code here](https://github.com/gahokas/grokking)
