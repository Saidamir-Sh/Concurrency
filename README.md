# Concurrency

> "Concurrency is a decoupling strategy. It helps us decouple what gets done from when it gets done." - 'Clean Code' by Robert C. Martin 

Concurrency is when two or more tasks can start, run, and complete in overlapping time periods. It doesn't necessarily mean they will ever both be running at the same instant. For example, multitasking on a single-core machine. However Concurrency is not Parallelism.

In example, Concurrency is two lines of customers ordering from a single cashier (lines take turns ordering); Paralleism is two lines of customers ordering from two cashiers (each line gets its own cashier);

#### To read more about differences between Parallelism and Concurrency, [here is more information](https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism) 

<br>

## Concurrency Defense Principles

### Single Responsibility Principles
Unfortunately, it is all too common for concurrency implementation details to be embedded directly into other production code. Here are a few things to consider:

Concurrency-related code has its own life cycle of development, change, and tuning.
Concurrency-related code has its own challenges, which are different from and often more difficult than nonconcurrency-related code.
The number of ways in which miswritten concurrency-based code can fail makes it challenging enough without the added burden of surrounding application code.
Recommendation: Keep your ocncurrency-related code separate from other code.

### Corollary: Limit the Scope of Data
The more places shared data can get updated, the more likely:

You will gorget to protect one or more of those places — effectively breaking all code that modifies that shared data.
There will be duplication of effort required to make sure everything is effectively guarded(violation of DRY)
It will be difficult to determine the source of failures, which are already hard enough to find.
Recommendation: Take data encapsulation to heart; severely limit the access of any data that may be shared.

### Corollary: Use Copies of Data
A good way to avoid shared data is to avoid sharing the data in the first place. In some situations it is possible to copy objects and treat them as read-only. In other cases it might be possible to copy objects, collect results from multiple threads in these copies and then merge the results in a single thread.

If there is an easy way to avoid sharing objects, the resulting code will be far less likely to cause problem. However, if using coppies of objects allows the code to avoid synchronizing, the savings in avoiding the intrinsic lock will likely make up for the additional creation and garbage collection overhead.

### Corollary: Threads Should Be as Independent as Possible
Consider writing your threaded code such that each thread exists in its own world, sharing no data with any other thread. each thread processes one client request, with all of its required data coming from an unshared source and stored as local variables. This makes each of those threads behave as if it were the only thread in the world and there were no synchronization requirements.

Recommendation: Attempt to partition data into independent subsets that can be operated on by independent threads, possibly in different processors.



