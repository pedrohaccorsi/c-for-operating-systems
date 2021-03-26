## Processes and programs

A `program` in Unix is a sequence of executable instructions. Apart from all its aspects, the only one that is of interest to us is the fact that a `program` is a sequence of instructions and data (on disk) that may potentially be executed at some point in time, maybe even multiple times, maybe even concurrently. 

`Processes`, in the other hand, are like a "structure" that wraps a <b>running</b> `program` and manages it, guarding its state, current directories, file handles, privileges and so on. You can run a `program` more than once, concurrently. For example, you can run two instances of a text editor, which edit two different texts. `program` and initial data are the same: it is the same editor. But the state inside each ``process`` is different: the text, the insert mode, cursor position and so on. From a programmers point of view, “the code is the same, but the variable values are different.

## User and Kernel mode

Whenever a Unix `process` does a system call (and at some other opportunities) the current `process` leaves the user context and the operating system code is being activated. This is privileged kernel code, and the activation is not quite a subroutine call, because not only is privileged mode activated, but also a kernel stack is being used and the CPU registers of the user `process` are saved. 

The kernel will then execute the system call on behalf of the user program, and then will try to exit the kernel. The typical way to leave the kernel is through the scheduler.

The scheduler will review the `process` list and current situation. It will then decide into which of all the different user `processes` to exit. It will restore the chosen `processes` registers, then return into this `processes` context, using this `processes` stack. The chosen `process` may or may not be the one that made the system call.

In short: Whenever you make a system call, you may (or may not) lose the CPU to another `process`.

That’s not too bad, because this other `process` at some point has to give up the CPU and the kernel will then return into our `process` as if nothing happened.

Our `program` is not being executed linearly, but in a sequence of subjectively linear segments, with breaks inbetween. During these breaks the CPU is working on segments of other `processes` that are also runnable.

## Visual representation

In the diagram below we can see that the entity ```program``` is contained inside the entity `process`. Also, we can see that different process can comunicate between each other freely. However, in order to comunicate with the hardware, user ```processes``` must make system calls and have Kernel do it for them.

![process structure](img/process-structure.svg)