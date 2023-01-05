# Serial vs Concurency Queue

Link: [https://www.avanderlee.com/swift/concurrent-serial-dispatchqueue/](https://www.avanderlee.com/swift/concurrent-serial-dispatchqueue/)

### What is a serial queue?

A serial Dispatch Queue performs only one task at the time. Serial queues are often used to synchronize access to a specific value or resource to prevent data races to occur.

```swift
let serialQueue = DispatchQueue(label: "swiftlee.serial.queue")

serialQueue.async {
    print("Task 1 started")
    // Do some work..
    print("Task 1 finished")
}
serialQueue.async {
    print("Task 2 started")
    // Do some work..
    print("Task 2 finished")
}

/*
Serial Queue prints:
Task 1 started
Task 1 finished
Task 2 started
Task 2 finished
*/
```

### What is a concurrent queue?

A concurrent queue allows us to execute multiple tasks at the same time. Tasks will always start in the order they’re added but they can finish in a different order as they can be executed in parallel. Tasks will run on distinct threads that are managed by the dispatch queue. The number of tasks running at the same time is variable and depends on system conditions.

```swift
let concurrentQueue = DispatchQueue(label: "swiftlee.concurrent.queue", attributes: .concurrent)

concurrentQueue.async {
    print("Task 1 started")
    // Do some work..
    print("Task 1 finished")
}
concurrentQueue.async {
    print("Task 2 started")
    // Do some work..
    print("Task 2 finished")
}

/*
Concurrent Queue prints:
Task 1 started
Task 2 started
Task 1 finished
Task 2 finished
*/
```



### Asynchronous vs synchronous tasks

A `DispatchQueue` task can be run synchronously or asynchronously. The main difference occurs when you create the task.

* Synchronously starting a task will block the calling thread until the task is finished
* Asynchronously starting a task will directly return on the calling thread without blocking
