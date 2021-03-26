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
  char *arg2 = "/home/pedro/Desktop/superprof/guilherme";

  execl(binaryPath, binaryPath, arg1, arg2, NULL);

  return 0;
}
```

Running this, we get:

```
pedro@ubuntu:~/Desktop/superprof/guilherme/aula-01/exec$ gcc ex01.c -o ex01
pedro@ubuntu:~/Desktop/superprof/guilherme/aula-01/exec$ ./ex01
total 12K
drwxrwxr-x 2 pedro pedro 4,0K mar 24 23:37 aula-00
drwxrwxr-x 5 pedro pedro 4,0K mar 26 14:38 aula-01
drwxrwxr-x 3 pedro pedro 4,0K mar 26 14:31 aula-02
```

 