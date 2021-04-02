## Generating signals via SHELL

We can send signals to a process via shell with the command `kill` . This command can send a given process any signal. For example, let's send the signal `SIGUSR1` to the process with `pid` 1234:

``` bash
$ kill -USR1 1234
```

Besides using kill to send signals to a process of our choose, we can also use some special key bindings to send signals to the foreground process we are executing. Some usual signals handled by default are:

| key binding | signal  |
|-------------|---------|
| Ctrl + C    | SIGINT  |
| Ctrl + \    | SIGQUIT |
| Ctrl + Z    | SIGTSTP |

## Generating via code

An easy way to send signals via code is with the function `kill()` defined in `<signal.h>`


```
int kill(pid_t pid, int sig);
```

This function will send the signal `sig` to the process with process id `pid`, and return an int to let the caller know what happened:

- `0` means at least one signal was sent
- `-1` means error, and `errno` is set to indicate the error.

Example:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>
#include <sys/wait.h>

int main(){

    pid_t fork_pid = fork();
    int status;

    if( fork_pid == 0){
        for(;;);
    }

    if(fork_pid > 0){
        kill(fork_pid, SIGTERM);
    }

    wait(&status);

    printf("Child process was terminated successfuly!\n");

    return 0;
}
```

Running this, we get:

```bash
pedro@ubuntu:~/Desktop/superprof/guilherme/aula-02/signal-handling$ gcc signal-example.c -o signal-example 
pedro@ubuntu:~/Desktop/superprof/guilherme/aula-02/signal-handling$ ./signal-example
Child process was terminated successfuly!
```


The code above will create a child process that will be stuck inside an infinite loop:

```c
if( fork_pid = 0){
    for(;;);
}
```

But not for too long, since the parent process will send a `SIGTERM` signal to the child, making it terminate immediately:

```c
if(fork_pid > 0){
    kill(fork_pid, SIGTERM);
}
```

And we can make sure that it works because we actually tell the parent signal to `wait` for the child. This means that if we didn't kill the child, the parent process would never reach the `printf()` line.


```c
wait();
printf("Child process was terminated successfuly!");
``` 

As it does, we can tell that the killing worked as expected.