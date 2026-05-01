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

[Your answer here - reference try-finally blocks, lock ordering, etc.]

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

**Which variables**: 

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

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
