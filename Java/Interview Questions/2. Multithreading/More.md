Here are **challenging and advanced interview questions** on multithreading to test a deep understanding of concepts. Each question comes with a detailed and clear explanation to make the answers meaningful and practical.

---

### **1. How does the `synchronized` keyword differ when applied to methods versus blocks?**

**Answer:**
- **Synchronized Method:**
  - Locks the entire method.
  - The lock is applied on the instance of the object (`this`) for **non-static methods** or the class object for **static methods**.
  - Drawback: Less granular, potentially reduces concurrency.
  ```java
  public synchronized void syncMethod() {
      // Entire method is synchronized
  }
  ```

- **Synchronized Block:**
  - Locks only the critical section of the code.
  - Allows more fine-grained control over which part of the code should be synchronized.
  ```java
  public void syncBlock() {
      synchronized(this) {
          // Critical section
      }
  }
  ```

**Question Insight:** Use synchronized blocks when only parts of the method need protection, reducing contention and improving performance.

---

### **2. What is thread starvation, and how can it occur?**

**Answer:**
- **Thread Starvation:**  
  A thread is perpetually waiting to acquire a resource because higher-priority threads or tasks dominate access to the resource.

- **Causes:**
  1. Threads with low priority may never get CPU time when high-priority threads monopolize the processor.
  2. Contention in synchronized blocks or locks, where one thread monopolizes the lock, preventing others from acquiring it.

- **Solution:**  
  Use **fair locks** or thread scheduling policies.
  ```java
  ReentrantLock lock = new ReentrantLock(true);  // Fair lock
  ```

---

### **3. How does Java prevent busy waiting?**

**Answer:**
Busy waiting occurs when a thread repeatedly checks for a condition without releasing CPU cycles, causing wastage. Java prevents this with:
1. **`wait()` and `notify()`:**
   - Threads release the lock and wait until notified.
   - Example:
     ```java
     synchronized(lock) {
         lock.wait();  // Releases lock and waits
         lock.notify();  // Wakes up waiting thread
     }
     ```

2. **BlockingQueues:**
   - Blocking queues (`ArrayBlockingQueue`, `LinkedBlockingQueue`) eliminate busy waiting by blocking threads when no elements are available.

3. **Locks with Timeouts:**
   - Use `tryLock()` with a timeout to avoid indefinite busy waiting.
   ```java
   if (lock.tryLock(5, TimeUnit.SECONDS)) {
       try { /* Critical Section */ }
       finally { lock.unlock(); }
   }
   ```

---

### **4. What is the purpose of `CyclicBarrier`, and how is it different from `CountDownLatch`?**

**Answer:**
- **CyclicBarrier:**
  - Allows multiple threads to wait for each other to reach a common barrier point.
  - After all threads arrive, they can proceed, and the barrier **resets automatically for reuse**.
  - Example:
    ```java
    CyclicBarrier barrier = new CyclicBarrier(3, () -> System.out.println("All threads ready!"));
    new Thread(() -> { barrier.await(); }).start();
    ```

- **CountDownLatch:**
  - Allows one or more threads to wait until the **count reaches zero**.
  - Count cannot be reset.
  - Example:
    ```java
    CountDownLatch latch = new CountDownLatch(2);
    new Thread(() -> { latch.countDown(); }).start();
    latch.await();
    ```

**Key Difference:**  
Use `CyclicBarrier` when the same barrier needs to be reused, and use `CountDownLatch` for one-time countdowns.

---

### **5. Why is the `Thread.sleep()` method a poor choice for thread synchronization?**

**Answer:**
- **Why It's Poor:**
  1. `Thread.sleep()` pauses a thread for a fixed time but doesn’t release any locks it holds.
  2. There's no guarantee the condition being waited for will be satisfied after the sleep duration.
  
- **Better Alternative:**
  Use `wait()`/`notify()` for conditional synchronization or `Semaphore`/`CountDownLatch`.

---

### **6. What is a livelock, and how is it different from a deadlock?**

**Answer:**
- **Deadlock:** Two or more threads are blocked indefinitely, each waiting for resources held by another thread.
- **Livelock:** Threads are not blocked but are perpetually changing their state in response to each other without making progress.

**Example of Livelock:**  
Two threads trying to back off simultaneously to avoid a conflict.
```java
while (true) {
    if (conditionToBackOff) {
        // Back off and try again
    }
}
```

**Solution:** Use **timeouts** or more robust algorithms to detect and break livelocks.

---

### **7. How does Java’s `ForkJoinPool` optimize parallelism?**

**Answer:**
- **Work Stealing:** Threads that finish their tasks can "steal" tasks from other threads' queues, ensuring all threads remain busy.
- **Task Splitting:** Breaks tasks into smaller subtasks recursively using the `RecursiveTask` or `RecursiveAction` classes.
  
**Example of Task Splitting:**
```java
ForkJoinPool pool = new ForkJoinPool();
pool.invoke(new RecursiveTask<Integer>() {
    protected Integer compute() {
        // Split and compute subtasks
        return 1 + 1;
    }
});
```

---

### **8. How would you handle a situation where one thread needs to wait for the completion of multiple threads?**

**Answer:**
Use **CountDownLatch**:
```java
CountDownLatch latch = new CountDownLatch(3);

new Thread(() -> {
    // Task 1
    latch.countDown();
}).start();

new Thread(() -> {
    // Task 2
    latch.countDown();
}).start();

latch.await();  // Main thread waits
System.out.println("All tasks completed!");
```

---

### **9. What is the purpose of a Daemon Thread in Java?**

**Answer:**
- A **Daemon Thread** is a low-priority background thread that serves other threads. The JVM automatically exits when only daemon threads are running.
- **Example:** Garbage Collector.
- **To Create a Daemon Thread:**
  ```java
  Thread daemon = new Thread(task);
  daemon.setDaemon(true);
  daemon.start();
  ```

---

### **10. How does a thread-safe Singleton work?**

**Answer:**
- **Double-Checked Locking:** Ensures thread safety without unnecessary locking.
  ```java
  class Singleton {
      private static volatile Singleton instance;

      private Singleton() {}

      public static Singleton getInstance() {
          if (instance == null) {
              synchronized (Singleton.class) {
                  if (instance == null) {
                      instance = new Singleton();
                  }
              }
          }
          return instance;
      }
  }
  ```

- **Volatile Keyword:** Prevents instruction reordering, ensuring the instance is fully initialized before being accessed by other threads.

---

### **11. What are Lock-Free Data Structures, and why are they useful?**

**Answer:**
- **Lock-Free Data Structures:**  
  Use atomic operations like CAS (Compare-And-Swap) instead of locks. Prevents thread blocking and reduces contention.
- **Examples:** ConcurrentLinkedQueue, ConcurrentHashMap.
- **Use Case:** High-performance systems like messaging queues.

---

These **challenging questions** cover advanced topics like synchronization, thread coordination, and Java’s concurrency framework. Would you like further deep-dives into a specific topic, more scenarios, or some hands-on examples?