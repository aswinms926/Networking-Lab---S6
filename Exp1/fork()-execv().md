# Understanding Fork and Execv in C

This project demonstrates the usage of two important system calls in C: `fork()` and `execv()`. The programs are split into two parts:
1. Demonstration of `fork()`.
2. Demonstration of `execv()` using two separate files.

## Fork Demonstration

### Code Overview
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <stdlib.h>

int main() {
     
    pid_t pid = fork();

     if (pid < 0) {
        printf("Fork Failed\n");
        exit(1);
    }

     if (pid == 0) {
        printf("Child Process:\n");
        printf("Child PID: %d\n", getpid());
        printf("Parent PID: %d\n", getppid());
        exit(0);
    }

     if (pid > 0) {
        printf("Parent Process:\n");
        printf("Parent PID: %d\n", getpid());
        printf("Child PID: %d\n", pid);

         wait(NULL);
        printf("Child Process Completed\n");
    }

    return 0;
}
```

### Explanation
1. **`fork()`**: Creates a new process (child process) by duplicating the calling process (parent process).
   - If `fork()` returns `0`, the current process is the child.
   - If `fork()` returns a positive value, it is the parent, and the return value is the PID of the child process.
2. **Child Process**:
   - Prints its own PID and its parent's PID using `getpid()` and `getppid()`.
3. **Parent Process**:
   - Prints its own PID and the PID of the child process.
   - Waits for the child process to complete using `wait(NULL)`.

### Output Example
```
Parent Process:
Parent PID: 12345
Child PID: 12346
Child Process:
Child PID: 12346
Parent PID: 12345
Child Process Completed
```

---

## Execv Demonstration

### Code Structure
1. **Main Program** (invokes `execv()`):
   ```c
   #include <stdio.h>
   #include <unistd.h>
   #include <stdlib.h>

   int main() {
        char *args[] = {"./child_program", NULL};
       
        execv(args[0], args);
       
        perror("execv");
       exit(1);
   }
   ```

2. **Child Program** (executed by `execv()`):
   ```c
   #include <stdio.h>

   int main() {
       printf("Child Program Executed Successfully!\n");
       printf("This is a simple execv() demonstration\n");
       
       return 0;
   }
   ```

### Explanation
1. **`execv()`**:
   - Replaces the current process image with a new process image.
   - In this example, it replaces the main program with `child_program`.
2. **Arguments**:
   - `args[0]` specifies the path to the program to execute (`./child_program`).
   - `args` is a `NULL`-terminated array of strings representing the arguments to the program.
3. **Error Handling**:
   - If `execv()` fails, it returns `-1`, and `perror()` is used to print the error message.

### Output Example
```
Child Program Executed Successfully!
This is a simple execv() demonstration
```

---

## How to Run the Programs

### Fork Demonstration
1. Save the `fork()` code in a file, e.g., `fork_example.c`.
2. Compile the program using:
   ```bash
   gcc fork_example.c -o fork_example
   ```
3. Run the program:
   ```bash
   ./fork_example
   ```

### Execv Demonstration
1. Save the main program code in a file, e.g., `execv_example.c`.
2. Save the child program code in a file, e.g., `child_program.c`.
3. Compile both programs:
   ```bash
   gcc execv_example.c -o execv_example
   gcc child_program.c -o child_program
   ```
4. Run the main program:
   ```bash
   ./execv_example
   ```

---

## Key Concepts
- **`fork()`**: Used for creating child processes.
- **`execv()`**: Used to replace a process with a new program.
- **`wait()`**: Ensures the parent waits for the child process to finish before continuing.

These programs showcase basic process management and interprocess communication techniques in C. They form the foundation for understanding advanced topics like multitasking and process synchronization.
