# APIs

There are many APIs in C to handle signals, the most known are:

| Lib     | function                |
|---------|-------------------------|
| ANSI-C  | signal()                |
| SystemV | sigset(), sysv_signal() |
| BSD     | sigvec(), bsd_signal()  |
| POSIX   | sigaction()             |


## POSIX

Let's focus our studies on the `POSIX` lib. The function `sigaction()` has the following syntax:

```c
int sigaction( int signum, 
               const struct sigaction *act, 
               struct sigaction *oldact )
```

The `sigaction` structure is defined as something like:


```c
struct sigaction {
    void     (*sa_handler)(int);                        //signal handler function
    void     (*sa_sigaction)(int, siginfo_t *, void *); //RT signal handler
    sigset_t   sa_mask;                                 //mask for blocked signals
    int        sa_flags;                                //mods for signal handling
    void     (*sa_restorer)(void);
};
```

### POSIX Example

```c
void signal_handler(int signal){
    printf("received the signal: %d\n", signal);
}

int maint(){

    struct sigaction signal_action;
    memset(&signal_action, 0, sizeof(signal_action));

    signal_action.sa_handler = &signal_handler;

    if( sigaction(SIGUSR1, &signal_action, NULL) ) != 0 ){
        perror("Fail on installing signal handler!");
        return -1;
    }

    for(;;);
    return 0;
}
```

The code above has 4 main steps:

1) declare (and implement) the signal handler function:

```c
void signal_handler(int signal){
    printf("received the signal: %d\n", signal);
}
```

2) Declare (and initialize) the `sigaction` structure.

```c
struct sigaction signal_action;
memset(&signal_action, 0, sizeof(signal_action));
```

3) Assign the function from step 1 into the structure of step 2

```c
signal_action.sa_handler = &signal_handler;
```

4) Attach the handler to a specific signal:

```c
sigaction(SIGUSR1, &signal_action, NULL)
```

And done!!Now if another process sends the signal `SIGUSR1` to this one, it will print the message `received the signal 10`;

