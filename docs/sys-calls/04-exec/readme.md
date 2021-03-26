So while `fork()` makes processes, `exec()` loads programs into processes that already exist.

`Exec()` has many variantes (`execl`, `execlp`, `execv`, `execvp`, `execle` and so on and so forth), here we will be taking a look at `execl`, but in general they all do the same.

## Execl 


```
int execl(const char *path, const char *arg, ..., NULL);
```


The function takes the path of the executable binary file (i.e. `/bin/ls`) as the first and second argument. Then, the arguments (i.e. `-lh`, `/home`) that you want to pass to the executable followed by `NULL`. Then `execl()` system function runs the command and load the target binary into the currnt process. If any error occurs, then `execl()` returns -1. Otherwise, it returns nothing and the new program starts running.

> Notice that all data related to the original program is completely lost.

In code:

```c
#include <unistd.h>
 
int main(void) {
  char *binaryPath = "/bin/ls";
  char *arg1 = "-lh";
  char *arg2 = "/home";
 
  execl(binaryPath, binaryPath, arg1, arg2, NULL);
 
  return 0;
}
```
If everything runs smoothly, the system should completely change the current process by the binaryPath sent to `execl()`. In other words, the `return 0` statement would never be reached.
 