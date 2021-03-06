```
void exit(int status)
```

The `fork()` system call is entered once, but left twice, and increments the number of processes in the system by one. After finishing our program the number of processes in the system is as large as before. That means there must be another system call which decrements the number of system calls. This system call is `exit()` .

`Exit()` is a system call you enter once and never leave. It decrements the number of processes in the system by one. It also accepts an exit status as a parameter, which the parent process can receive (or even has to receive), and which communicates the fate of the child to the parent.

`Exit()` closes all files and sockets, frees all memory and then terminates the process. The parameter of `exit()`  is the only thing that survives and is handed over to the parent process.