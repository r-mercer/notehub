# Concurrency and Parallelism

## Definitions

Concurrency: When two or more tasks can start and complete in overlapping time periods. Does not require that they run at the same time. This is like multitasking on a single core.

Parallelism: When two tasks literally run at the same time. Multicore processing.

Parallel Array: Also known as structure of an array, parallel arrays are multiple arrays that are all the same size, in which each element of the array is closely related to the items at the corresponding index in the other arrays.

### Parallel Arrays

They can be used in languages which support only arrays of primitive types and not of records (or perhaps don't support records at all).
Parallel arrays are simple to understand and use, and are often used where declaring a record is more trouble than it's worth.
They can save a substantial amount of space in some cases by avoiding alignment issues.
If the number of items is small, array indices can occupy significantly less space than full pointers, particularly on architectures with large words.
Sequentially examining a single field of each record in the array is very fast on modern machines, since this amounts to a linear traversal of a single array, exhibiting ideal locality of reference and cache behavior.

---

# Tasks

Tasks live in `System.Threading.Tasks`

Tasks own actions

Tasks are naturally multithreaded

When using slanty braces to indicate a type, you are declaring a generic type. Generic types appear everywhere.

Getting the result of a task is a blocking operation

## Task Cancellation

Cancellation tokens are things that exist. Not massively sure what the impact of this is. Maybe if you have a constantly executing tasks?

`Throw New OperationCanceledException` will exit task and not report all of the way up to the user

You can also link cancellation tokens together and handle them together with `CreateLinkedTokenSource`

## Exceptions

You can actually aggregate all of the exceptions and then report them all at once which is kinda sick

Exceptions in multithreaded operations are only breaking if you are observing them

## Locks

Used to ensure threadsafe things are threadsafe. Actually not sure if I need to reeeaaaaallly worry about this just yet.

Spin locks

Mutex; lets you try and get a lock but will wait for the ability to do so?

Reader writer locks let you READ from multiple locks at once, but only WRITE on the single thread

- `ReaderWriterLockSlim` is class

## Threadsafe Lists

### Concurrent Collections

#### Concurrent Dictionary

Similar to other types of collection, but is a liiiiitle different.

- Exceptions normally triggered by `.add`, but exceptions are only effective in multithreaded situations if they are observed

#### Concurrent Bag

What on earth is a bag.

Bags of elements are fast, provided that you do not care about their ordering??

## Parallel LINQ

Default LINQ queries are sequential; but there are parallel options, namely PLINQ.

You just call `EnumerableVar.AsParallel()` and BAM multithreaded

`.ForAll()` not something else though?

`.AsOrdered()` will force ordered multithreading if that is needed

There are also parallel enumerables, which is similar to calling `AsParallel` on a thing, but will also ensure that the resulting calls are done in parallel!

## Await

Await is basically a way of declaring "The code that follows me is a continuation".

## Additional Notes

- Without configuring `MaxDegreeOfParallelism()`, .NET will default to using all system threads. This is okay if you only have the one call, but if you have two unconfigured Parallel loops running, then they will fight with each other for the resources.
- Concurrent Queues are threadsafe on their own.
- If you are using a `Task.Factory` you should probably configure the scheduler arguments.
- Task Processing Library is the money and is where I should be focusing my energy.
- Producer and Consumer Interface is probably a bit much and probably shouldn't worry about it that much.
