### Protocol (Initial idea)
- head and tail as AtomicReference objects due to both having potential concurrency issues while being modified as intended
- "*first*" and "*last*" variables for copies of head and tail, CAS to try and set last.next
- dequeuer is supposed to retry til failure, enqueuer does not attempt any retries
