# Assignment 3 - Complete Documentation

**Student Name**: [Your Full Name]  
**Student ID**: [Your ID]  
**Date Submitted**: [Submission Date]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [3 May 2026, 4:30 PM]
**What I implemented**: I added synchronization to the scheduler code. I used ReentrantLock to protect shared variables like counters, and I used a semaphore to control CPU access so only one thread runs at a time.

**Challenges encountered**: I faced problems with threads affecting each other and giving wrong results. Also, I was worried about forgetting to release locks which might cause errors.

**How I solved it**: I used try-finally with the locks to make sure they always unlock. I also used a semaphore with 1 permit to make sure only one thread uses the CPU.

**Testing approach**: I ran the program several times and checked if the results are consistent. I also checked that there are no errors during execution.

**Time spent**: 2 hours

---

### Entry 2 - [3 May 2026, 9:00 PM]
**What I implemented**: Started working on protecting shared variables. I added ReentrantLock to some counters in the code.

**Challenges encountered**: At first I was confused where exactly to put the lock and unlock, and sometimes the program didn’t behave correctly.

**How I solved it**: I reviewed the code and focused on the critical sections only. I made sure to lock before updating shared variables and unlock after.

**Testing approach**: I ran the program a few times and checked if values are changing correctly without errors.

**Time spent**: 1.5 hours

---

### Entry 3 - [4 May 2026, 6:30 PM]
**What I implemented**: I worked on controlling access to the CPU using a semaphore.

**Challenges encountered**: I wasn’t sure how many permits to use and where to place acquire and release.

**How I solved it**: I used a binary semaphore (1 permit) and placed acquire before execution and release after finishing.

**Testing approach**: I tested by running multiple threads and checked that only one thread is executing at a time.

**Time spent**: 1 hour

---

### Entry 4 - [[4 May 2026, 8:30 PM]
**What I implemented**: Improved the code and fixed small issues. Also made sure the execution log is protected to avoid errors.

**Challenges encountered**: Sometimes I got issues when multiple threads tried to write to the log at the same time.

**How I solved it**: I added a lock around the log operations to make it thread-safe.

**Testing approach**: I ran the program several times and confirmed there are no exceptions and results are stable.

**Time spent**: 1 hour

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[One race condition happens with shared counters like contextSwitchCount and completedProcessCount. These variables are accessed by multiple threads, and if they update them at the same time (like in incrementContextSwitch()), the value may become incorrect. This is because incrementing is not an atomic operation, so some updates can be lost.

Another race condition occurs when threads write to the executionLog (ArrayList). If multiple threads call logExecution() at the same time, it can cause inconsistent data or runtime errors. This may result in missing or incorrect log entries during execution.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[ReentrantLock is used to protect shared data by allowing only one thread to access a critical section at a time. Semaphore is used to control how many threads can access a resource simultaneously. In my code, I used ReentrantLock for shared variables like contextSwitchCount, completedProcessCount, and executionLog to prevent race conditions. I used a semaphore with 1 permit (cpuSemaphore) to control CPU access so only one process runs at a time. This helps keep the execution organized and prevents conflicts between threads.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Deadlock is a situation where two or more threads are stuck waiting for each other and none of them can continue. One prevention technique is using try-finally to make sure locks are always released even if an error happens. Another technique is avoiding holding multiple locks at the same time to reduce circular waiting.

In my code, I used try-finally blocks to always release the locks after using them. I also made sure each method uses only one lock at a time and releases it quickly. This helps prevent deadlocks and keeps the program running correctly.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**: These variables are shared between multiple threads, and each thread updates them during execution. Without protection, concurrent updates can cause incorrect values or lost updates.

**Synchronization mechanism used**: ReentrantLock (separate lock for each variable)

**Code snippet**:
```java
public static void incrementContextSwitch() {
    contextSwitchLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        contextSwitchLock.unlock();
    }
}

public static void incrementCompletedProcess() {
    completedProcessLock.lock();
    try {
        completedProcessCount++;
    } finally {
        completedProcessLock.unlock();
    }
}

public static void addWaitingTime(long time) {
    waitingTimeLock.lock();
    try {
        totalWaitingTime += time;
    } finally {
        waitingTimeLock.unlock();
    }
}
```

**Justification**: I used ReentrantLock to ensure only one thread updates each variable at a time. This prevents race conditions and guarantees correct results. Using separate locks also reduces blocking between threads.

---

### Critical Section #2: Execution Log

**What resource**: The shared resource is executionLog, which is an ArrayList used to store log messages from all threads.

**Why it needs protection**: Because multiple threads may try to add messages at the same time, which can cause data corruption or ConcurrentModificationException.

**Synchronization mechanism used**: I used ReentrantLock to protect the execution log.

**Code snippet**:
```java
public static final ReentrantLock logLock = new ReentrantLock();

public static void logExecution(String message) {
    logLock.lock();
    try {
        executionLog.add(message);
    } finally {
        logLock.unlock();
    }
}
```

**Justification**: I used ReentrantLock to make sure only one thread can access the log at a time. This prevents race conditions and keeps the log correct and safe.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: The semaphore is used to control access to the CPU and make sure that only one process runs at a time.


**Number of permits and why**: I used 1 permit because the CPU can only execute one process at a time (like a single-core CPU).


**Where implemented**: It is implemented inside the run() method and runToCompletion() method before the process starts execution.

**Code snippet**:
```java
public static final Semaphore cpuSemaphore = new Semaphore(1);

@Override
public void run() {
    try {
        SharedResources.cpuSemaphore.acquire();
        try {
            // process execution code
        } finally {
            SharedResources.cpuSemaphore.release();
        }
    } catch (InterruptedException e) {
        System.out.println("Semaphore interrupted");
    }
}
```

**Effect on program behavior**: The semaphore makes sure processes do not run at the same time. This prevents conflicts and keeps execution organized and correct.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: I ran the program several times using VS Code run button. Each time I observed the output such as context switches, completed processes, and waiting time.
```bash
I ran the program 5 times using the Run button in VS Code.
```

**Results**: 
(The results were consistent in all runs.

* All processes completed successfully
* No missing or duplicated data
* Context switch count and waiting time were logical
* No errors or crashes happened)

**Why synchronization is necessary**: 
(Without synchronization, multiple threads could access shared variables at the same time. This may cause race conditions such as:

* Incorrect contextSwitchCount (wrong counting)
* Wrong totalWaitingTime values
* Errors in executionLog like missing or corrupted data

These shared resources need protection because they are updated by multiple threads concurrently.)

**Conclusion**: Synchronization ensures correct and stable results. It prevents race conditions and makes the program safe when multiple threads are running.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: I ran the program multiple times and monitored the execution log while multiple threads were adding messages at the same time. I focused on the part where executionLog is updated by different threads.


**Results**: No ConcurrentModificationException occurred in any run. The program executed normally and all log messages were added correctly without errors.


**What this proves**: This proves that using ReentrantLock to protect the executionLog works correctly. It ensures that only one thread accesses the list at a time, which prevents concurrent modification problems.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
1- All processes should finish execution
2- completedProcessCount should equal number of processes
3- contextSwitchCount should increase logically (based on number of quanta)
4- totalWaitingTime should be a positive and reasonable value

**Actual values**: 
* All processes completed successfully
* completedProcessCount matched the total number of processes
* contextSwitchCount was consistent with execution flow
* totalWaitingTime was calculated correctly without negative or incorrect values

**Analysis**: The results matched the expected values. This shows that the synchronization mechanisms are working correctly. All shared variables were updated safely without errors, and the program produced accurate final results.

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
