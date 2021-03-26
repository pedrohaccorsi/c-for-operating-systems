```
pid_t wait(int *wstatus);
```

Our child process ends with an `exit(0)`. The 0 is the exit status of our program and can be shipped. We need to make the parent process pick up this value and we need a new system call for this.

This system call is `wait()`.

In Code:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
 
int main() {
        pid_t pid = 0;
        int   status;
 
        pid = fork();
        if (pid == 0) {
                printf("I am the child.\n");
                sleep(10);
                printf("I am the child, 10 seconds later.\n");
        }
        if (pid > 0) {
                printf("I am the parent, the child is %d.\n", pid);
                pid = wait(&status);
                printf("End of process %d: ", pid);
                if (WIFEXITED(status)) {
                        printf("The process ended with return code(%d).\n", WEXITSTATUS(status));
                }
                if (WIFSIGNALED(status)) {
                        printf("The process ended with kill -%d.\n", WTERMSIG(status));
                }
        }
        if (pid < 0) {
                perror("In fork():");
        }
 
        return 0;
}
```

Running this, we get:


```
pedro@ubuntu:~/Desktop/superprof/guilherme/aula-01/wait$ gcc ex01.c -o ex01
pedro@ubuntu:~/Desktop/superprof/guilherme/aula-01/wait$ ./ex01
I am the parent, the child is 4064.
I am the child.
I am the child, 10 seconds later.
End of process 4064: The process ended with return code (0).
```

The variable status is passed to the system call `wait()` as a reference parameter, and will be overwritten by it. The value is a bitfield, containing the exit status and additional reasons explaining how the program ended. To decode this, C offers a number of macros with predicates such as `WIFEXITED()` or `WIFSIGNALED()`. We also get extractors, such as `WEXITSTATUS()` and `WTERMSIG()`. `wait()` also returns the pid of the process that terminated, as a function result.

`Wait()` stops execution of the parent process until either a signal arrives or a child process terminates. You can arrange for a `SIGALARM` to be sent to you in order to time bound the `wait()`.