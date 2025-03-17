### **What is a Thread in Java?**
A **thread** in Java is a lightweight process that executes independently. Java provides built-in support for multithreading, allowing concurrent execution of two or more parts of a program for maximum CPU utilization.

---

### **How to Create a Thread in Java?**
There are two ways to create a thread in Java:

#### **1. Extending the `Thread` class**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // Starts the thread, calling run() internally
    }
}
```

#### **2. Implementing the `Runnable` interface (Recommended)**
```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        Thread t1 = new Thread(new MyRunnable());
        t1.start(); // Starts the thread
    }
}
```
> The **Runnable interface** is preferred because it allows you to extend another class.

---

### **Thread Lifecycle (States)**
A thread in Java has five states:

1. **NEW** – Created but not started (`Thread t = new Thread();`)
2. **RUNNABLE** – Ready to run, waiting for CPU (`t.start();`)
3. **BLOCKED** – Waiting for a monitor lock (e.g., waiting to enter a synchronized block)
4. **WAITING** – Waiting indefinitely for another thread’s signal (`t.wait();`)
5. **TIMED_WAITING** – Waiting for a specified time (`Thread.sleep(1000);`)
6. **TERMINATED** – Completed execution or stopped

---

### **Thread Methods**
- `start()` – Starts a new thread.
- `run()` – Contains the code that runs in the thread.
- `sleep(ms)` – Pauses execution for `ms` milliseconds.
- `join()` – Waits for a thread to finish.
- `yield()` – Suggests that the thread scheduler give other threads a chance.
- `interrupt()` – Interrupts a thread.

---

### **Thread Synchronization**
When multiple threads access shared resources, **synchronization** is required to avoid race conditions.

#### **Using `synchronized` Keyword**
```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```
> **Synchronization ensures** that only one thread can execute a synchronized method at a time.

#### **Using `ReentrantLock` (Better control)**
```java
import java.util.concurrent.locks.ReentrantLock;

class Counter {
    private int count = 0;
    private final ReentrantLock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }
}
```

---

### ** Thread Communication (Passing Messages between Threads)**
Threads communicate using **wait(), notify(), and notifyAll()** methods. These methods must be used inside synchronized blocks to avoid race conditions.

#### **Example of Inter-Thread Communication using wait() and notify()**
```java
class SharedResource {
    private boolean dataReady = false;

    synchronized void produce() throws InterruptedException {
        System.out.println("Producing data...");
        Thread.sleep(2000);
        dataReady = true;
        notify(); // Notify the waiting thread
    }

    synchronized void consume() throws InterruptedException {
        while (!dataReady) {
            wait(); // Wait until producer notifies
        }
        System.out.println("Consuming data...");
    }
}

public class Main {
    public static void main(String[] args) {
        SharedResource resource = new SharedResource();

        Thread producer = new Thread(() -> {
            try { resource.produce(); } catch (InterruptedException e) { }
        });

        Thread consumer = new Thread(() -> {
            try { resource.consume(); } catch (InterruptedException e) { }
        });

        consumer.start(); // Start consumer first to ensure it waits
        producer.start(); // Then start producer
    }
}
```
#### **Explanation:**
1. The `consumer` waits using `wait()` until the producer produces data.
2. The `producer` notifies the `consumer` using `notify()` once data is ready.

---

---

### **Multithreading Issues & Solutions**
| Issue | Solution |
|--------|-------------|
| **Race Condition** (Multiple threads modifying shared data) | Use **synchronized** blocks or **Locks** |
| **Deadlock** (Two threads waiting on each other indefinitely) | Avoid nested locks, use **tryLock()** |
| **Starvation** (Low-priority thread never gets CPU time) | Use **fair locks** |
| **Livelock** (Threads keep responding to each other but make no progress) | Use **timeouts** |

---

### **Executors (Thread Pool)**
Instead of creating multiple threads manually, Java provides the **Executor Framework** to manage threads efficiently.

```java
import java.util.concurrent.*;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(5);

        for (int i = 1; i <= 10; i++) {
            executor.execute(() -> System.out.println("Task executed by " + Thread.currentThread().getName()));
        }

        executor.shutdown();
    }
}
```
> **Thread pools help** in reusing threads and improving performance.

---

### **Final Tips for Interview**
1. **Understand the difference** between `Thread` class and `Runnable` interface.
2. **Know how to handle shared resources** using `synchronized`, `Lock`, and `volatile`.
3. **Be prepared to explain deadlock, race condition, and starvation.**
4. **Be familiar with thread-safe collections** (`ConcurrentHashMap`, `CopyOnWriteArrayList`).
5. **Know how to use `Executors` for efficient thread management.**

---

Would you like me to add sample questions you might be asked in the interview? 🚀

---

By default, the `run()` method in Java is **asynchronous**.  

### **Explanation:**
- When you call `start()`, Java creates a **new thread** and executes the `run()` method **asynchronously** in that thread.
- If you call `run()` directly instead of `start()`, it executes **synchronously** in the main thread (like a normal method call).

### **Example: Asynchronous Execution**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println(Thread.currentThread().getName() + " is running");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();

        t1.start();  // Runs in a separate thread (asynchronous)
        t2.start();  // Runs in another separate thread (asynchronous)
    }
}
```
#### **Output (may vary due to multithreading)**
```
Thread-0 is running
Thread-1 is running
```
> Threads execute independently and asynchronously.

---

### **Example: Synchronous Execution**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println(Thread.currentThread().getName() + " is running");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();

        t1.run();  // Runs in the main thread (synchronous)
        t2.run();  // Runs in the main thread (synchronous)
    }
}
```
#### **Output (Always same order)**
```
main is running
main is running
```
> Here, `run()` is just a normal method call, so it runs **synchronously** in the **main thread**.

---

### **How to Make `run()` Synchronized?**
If you want to make `run()` synchronized so that only one thread executes it at a time, use the `synchronized` keyword.

```java
class MyThread extends Thread {
    public synchronized void run() {
        System.out.println(Thread.currentThread().getName() + " is running");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();

        t1.start();
        t2.start();
    }
}
```
> Now, only one thread will execute `run()` at a time.

---

### **Conclusion:**
- **By default, `run()` is asynchronous when using `start()`.**
- **If you call `run()` directly, it runs synchronously in the same thread.**
- **To make `run()` synchronized, use the `synchronized` keyword.** 🚀

Would you like me to provide interview questions related to this?
