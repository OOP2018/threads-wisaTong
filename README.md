## Threads and Synchronization

This lab illustrates the problem of synchronization when many threads are operating on a shared object.  The general issue is called "thread safety", and it is a major cause of errors in computer software.

## Assignment

To the problems on the lab sheet and record your answers here.

1. Record average run times.
2. Write your explanation of the results.  Use good English and proper grammar.  Also use good Markdown formatting.

## ThreadCount run times

These are the average runtime using 3 or more runs of the application.
The Counter class is the object being shared by the threads.
The threads use the counter to add and subtract values.

| Counter class           | Limit              | Runtime (sec)   |
|:------------------------|:-------------------|-----------------|
| Unsynchronized counter  | 10000000           | 0.017350        |
| Using ReentrantLock     | 10000000           | 0.738168        |
| Syncronized method      | 10000000           | 0.869868        |
| AtomicLong for total    | 10000000           | 0.196656        |

## 1. Using unsynchronized counter object

answer the questions (1.1 - 1.3)

1.1 It is not the same each time.

1.2 Record the total 3 time
	* -49333273521435
	* 28801955681392
	* -49988123088883
		* average -2.350648E+13

1.3  The total is not always the same, because we share a mutable object with multiple threads and each thread run on its own and when multiple thread read values from the object and use the value for computation and then write the value back to the same mutalble object if multiple thread is reading and use the same value for computation when the thread finish computing they write back the slower thread will override the result causing the error.

## 2. Implications for Multi-threaded Applications

How might this affect real applications?  

If there are 2 or more financial activities happen at the same time then maybe there will be some error that is unpredictable, and sure enough we don't want to have an error in a real application especially unpredictable one.

## 3. Counter with ReentrantLock

answer questions 3.1 - 3.4

3.1 The result is always 0. average runtime is 0.738168 sec.

3.2 The result is different from problem 1 because now the method is locked by the Lock interface until everything in the try finally block is executed, everything in the block is locked and any other thread cannot access it so other threads need to wait for the computation to finish before reading and writing the value to the variable.

3.3 ReentrantLock lock the code if another thread access that code pause that thread until unlock() has been invoked.

When and why? We want to use this when we want to carefully monitor a shared variable among various thread because wen don't threads to read and write to the same variable at the same time.

3.4 We write "finally" in the code to state that until this String of activities is done the code is lock and it will be unlock when everything is done so the code is now accessible.

## 4. Counter with synchronized method

answer question 4 

4.1 The total is 0. average runtime is 0.869868 sec.

4.2 The result is different from problem 1 bacause the same reason as using ReentrantLock

4.3 The "Synchronized" means that every threads working synchronously waiting for another thread to finsih doing something and then the thread can start doing it.

## 5. Counter with AtomicLong

answer question 5

5.1 This AtomicLong does a "atomic" operation this operation use compare-and-swap(CAS) to ensure data integrity everything is compute normally until the data is need to be written if the cumputed value is not matches the existing expected value then the computed value is discarded and start the computation again.

5.2 We want to use this solution when we write a simple program with a shared variable that will be working with multiple thread and this solution is FAST but in a more complicated scenario this method might not work in my opinion.

## 6. Analysis of Results

answer question 6

6.1 The fasteset solution is AtomicCounter. The slowest solution is Synchronized method.

6.2 The Lock solutions an be apllied to the broadest range of problem because we can define the scope of what is locked when the operation is ocurring with other solution we cannot define the scope. For example if we have a complicate method which does a lot of computation creating and get values from the objects to perform some task if we declared that me thod synchronized then we cannot let other threads do anything that will not have any side effects. 

## 7. Using Many Threads (optional)

