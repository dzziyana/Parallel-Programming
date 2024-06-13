A monitor is not an object, it’s an interface
Any code object that has the following functionality is a monitor...

## Functionality
A monitor provides a mechanism to check a condition with following semantics:
If a condition does not hold...
- release the monitor lock
- wait for the condition to become true
- signaling mechanism to avoid busy-loops (spinning)

## In Java: wait-notify
[[ExecutorService (WIP)]]

