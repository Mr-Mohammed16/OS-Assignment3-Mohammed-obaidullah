# Assignment 3 - Complete Documentation

**Student Name**: [Mohammed Obaidullah Al-Ghamdi]  
**Student ID**: [445050131]  
**Date Submitted**: [May 2 ]

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

### Entry 1 - [ May 2 , 2026 (2:00 )]
**What I implemented**: I forked the starter repository, updated my Student ID to 445050131, and reviewed the original code to identify the critical sections where race conditions might occur

**Challenges encountered**: Understanding how multiple threads were modifying the shared counters simultaneously.

**How I solved it**: I mapped out all global variables in `SharedResources` that needed synchronization.

**Testing approach**: Ran the base code to observe the inconsistent results and exceptions.

**Time spent**: 1.5 hours

---

### Entry 2 - [ May 2, 2026 (3:30 )]
**What I implemented**: Implemented Task 1 by adding a `ReentrantLock` named `statsLock` to protect the counter variables (`contextSwitchCount`, `completedProcessCount`, `totalWaitingTime`).

**Challenges encountered**: Deciding whether to use one lock for all counters or separate locks.

**How I solved it**: I opted for a single lock (`statsLock`) for simplicity and implemented the `try-finally` block to ensure locks are always released.

**Testing approach**: Ran the program multiple times to ensure counters updated consistently without lost increments.

**Time spent**: 2 hours

---

### Entry 3 - May 2, 2026 (5:00 )
**What I implemented**:Implemented Task 2. I added a separate `ReentrantLock` named `logLock` to protect the `executionLog` ArrayList.

**Challenges encountered**: The ArrayList is not thread-safe and was throwing `ConcurrentModificationException` during multithreaded operations.

**How I solved it**: I wrapped the `executionLog.add()` method inside a `try-finally` block controlled by `logLock`.

**Testing approach**: Ran the simulation with 20 processes to verify that the log captured all events without crashing.

**Time spent**: 1.5 hours 


---

### Entry 4 - May 2, 2026 (6:30 )
**What I implemented**: Implemented Task 3. I added a `Semaphore(1)` to control access to the CPU execution block inside the `run()` method of the Process class.

**Challenges encountered**: Ensuring that a process properly yields the CPU and releases the semaphore even if interrupted.

**How I solved it**: I used `cpuSemaphore.acquire()` before the execution logic and placed `cpuSemaphore.release()` inside the `finally` block of the `run()` method.

**Testing approach**: Monitored the console output to ensure that only one process was executing its quantum at any given millisecond.

**Time spent**: 2 hours

---

### Entry 5 - May 2, 2026 (07:00 )
**What I implemented**: Finalized the documentation, verified GitHub commits, and recorded the video demonstration.

**Challenges encountered**: Keeping the video explanation concise under 5 minutes while covering all requirements.

**How I solved it**: Wrote a script beforehand to smoothly transition from code walkthrough to program execution.

**Testing approach**: Tested the video link in incognito mode.

**Time spent**: 2 hours

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

1. Counter Updates (`contextSwitchCount++`)**: The shared resource is the integer counter. Concurrent access is a problem because `++` is not an atomic operation (it involves reading, incrementing, and writing). If two threads read the same value simultaneously, one increment will be overwritten and lost, resulting in an incorrect lower total count.
2. Adding to Execution Log (`executionLog.add()`)**: The shared resource is the `ArrayList`. Concurrent access is dangerous because `ArrayList` dynamically resizes. If multiple threads try to add elements simultaneously, the internal array indices might overlap, leading to a `ConcurrentModificationException` or data loss.

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[A `ReentrantLock` provides mutual exclusion, meaning only the thread that acquired the lock can release it. I used it for the `statsLock` and `logLock` to protect critical sections modifying shared data (counters and lists). A `Semaphore` is a signaling mechanism based on permits; it restricts how many threads can access a resource simultaneously. I used a Binary Semaphore (`Semaphore(1)`) to act as a gatekeeper for the CPU, ensuring only one process enters the execution state at a time.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[A deadlock is a situation where two or more threads are blocked indefinitely, each waiting for a resource held by another. 
Two prevention techniques are: 
1) Lock Ordering: Acquiring multiple locks in a strict, predefined order across all threads.
2) Guaranteed Release: Ensuring locks are released even if an error occurs.
In my code, I prevented deadlocks by strictly using the `try-finally` block. I placed `statsLock.unlock()`, `logLock.unlock()`, and `cpuSemaphore.release()` inside `finally` blocks, guaranteeing that the resources are freed even if the thread throws an `InterruptedException`.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[I chose a coarse-grained locking approach by using a single lock (`statsLock`) to protect all three counters (`contextSwitchCount`, `completedProcessCount`, `totalWaitingTime`). I made this choice because it simplifies the code and reduces the risk of deadlocks that might occur from managing multiple nested locks. However, the trade-off is reduced concurrency; if one thread is updating the context switch counter, another thread cannot update the total waiting time simultaneously. Since these three counters are completely independent, a fine-grained approach (one separate lock for each counter) would actually provide better concurrency, allowing non-conflicting updates to happen in parallel.]

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
