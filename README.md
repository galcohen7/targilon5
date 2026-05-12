# targilon5

https://github.com/galcohen7/targilon5.git

# Task 3:
When running the code without any synchronization, the final output of `bar` is less than 
20,000 and will be different in each time. 

The reason it happens is because the operation `bar++` is not atomic- meaning It consists of few separate steps in the CPU: 
* Reading the current value from memory.
* Incrementing the value by 1.
* Writing the updated value back in memory.

If 2 (or more) of the threads execute without protection, they can read the same old value at the same time, increment it, and overwrite each other's progress. And it will lead to us losing part of the updates

# Task 4:
We added the `synchronized` keyword to the methods `baz()` and `getBar()`. This locks the whole method so only one thread can enter at a time. It prevents the interleaving, fixes the race condition, and makes the final output exactly 20,000 every time.

# Task 5:
Instead of synchronizing the whole method, we used a `synchronized(this)` block only around the line `bar++`. This does the same job of fixing the race condition and ensuring the output is 20,000, but it limits the lock for the specific line that needs protection.

# Task 6:
We ran 10 threads with 10,000,000 iterations each without any synchronization:
The execution speed is very fast since there were no locks slowing it down and the threads don't need to wait for each other.
The output was way below 100,000,000 because the high number of loops caused constant race conditions, and many updates were lost.

# Task 7:
We ran the same test but with the `synchronized(this)` block back in code:
The output was correct- exactly 100,000,000, because the lock prevented the threads from overwriting each other's updates.
The runtime increased by alot because of the extra work the OS has to do to manage locks, handle thread competition, and switch between threads.
