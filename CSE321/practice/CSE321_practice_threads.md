# Practice Problems on POSIX Threads

Code analysis practice problems exploring POSIX thread creation, argument passing, sequential join vs concurrent execution, and global variable modification.
Docx last modified: 07-11-2025.

## Content

### Problem 1: Parameterized Thread Execution with Sequential Join
Two threads created sequentially with an integer array argument `[thread_id, delta]` modifying global `var = 0`. Thread 1 adds 5 (`var = 5`), then finishes; Thread 2 subtracts 3 (`var = 2`). Final `var = 2`.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>

void *t_func(void *arg);
int var = 0;
int t_id[] = {1, 2};

int main(){
    pthread_t t1;
    pthread_t t2;
    int a1[] = {t_id[0], 5};
    int a2[] = {t_id[1], 3};
    pthread_create(&t1, NULL, t_func, (void *)a1);
    pthread_join(t1, NULL);
    pthread_create(&t2, NULL, t_func, (void *)a2);
    pthread_join(t2, NULL);
    printf("Value of var after operations of threads: %d
", var);
    return 0;
}

void *t_func(void* arg){
    int* x = arg;
    if(x[0] == 1){
        printf("Entered in Thread :%d
", x[0]);
        var += x[1];
        printf("Value of var after the operation of Thread %d: %d
", x[0], var);
        printf("Operation Done by Thread %d...
", x[0]);
    }
    else{
        printf("Entered in Thread :%d
", x[0]);
        var -= x[1];
        printf("Value of var after the operation of Thread %d: %d
", x[0], var);
        printf("Operation Done by Thread %d...
", x[0]);
    }
}
```

---

### Problem 2: Loop Thread Creation with Immediate Join
Three threads created and joined one-by-one in a loop with `t_id = {1, 2, 3}` and `var = 50`.
- Thread `*v == 1`: subtracts 4 three times ($50 - 12 = 38$).
- Thread `*v == 2`: multiplies by 2 three times ($38 \times 8 = 304$).
- Thread `*v == 3`: multiplies by 2 three times ($304 \times 8 = 2432$).
- Final `var = 2432`.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>

int t_id[] = {1, 2, 3};
int var = 50;
void *t_func(int *v);

int main(){
    pthread_t t[3];
    for(int i = 0; i < 3; i++){
        pthread_create(&t[i], NULL, (void *)t_func, &t_id[i]);
        pthread_join(t[i], NULL);
    }
    printf("Final value of var: %d
", var);
    return 0;
}

void *t_func(int *v){
    if(*v == 0){
        printf("Entered in Thread %d...
", *v);
        for(int i = 0; i < 3; i++){
            var += 5;
            printf("Thread %d modified value %d
", *v, var);
        }
        printf("Modification done by Thread %d, value %d
", *v, var);
    }
    else if(*v == 1){
        printf("Entered in Thread %d...
", *v);
        for(int i = 0; i < 3; i++){
            var -= 4;
            printf("Thread %d modified value %d
", *v, var);
        }
        printf("Modification done by Thread %d, value %d
", *v, var);
    }
    else{
        printf("Entered in Thread %d...
", *v);
        for(int i = 0; i < 3; i++){
            var *= 2;
            printf("Thread %d modified value %d
", *v, var);
        }
        printf("Modification done by Thread %d, value %d
", *v, var);
    }
}
```

---

### Problem 3: Concurrent Thread Creation with Batch Join
Three threads created concurrently in a first loop and then all joined in a second loop with shared `var = 50`. Demonstrates concurrent execution and race condition nondeterminism without synchronization.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>

int t_id[] = {1, 2, 3};
int var = 50;
void *t_func(int *v);

int main(){
    pthread_t t[3];
    for(int i = 0; i < 3; i++){
        pthread_create(&t[i], NULL, (void *)t_func, &t_id[i]);
    }
    for(int i = 0; i < 3; i++){
        pthread_join(t[i], NULL);
    }
    printf("Final value of var: %d
", var);
    return 0;
}

void *t_func(int *v){
    if(*v == 0){
        printf("Entered in Thread %d...
", *v);
        for(int i = 0; i < 3; i++){
            var += 5;
            printf("Thread %d modified value %d
", *v, var);
        }
        printf("Modification done by Thread %d, value %d
", *v, var);
    }
    else if(*v == 1){
        printf("Entered in Thread %d...
", *v);
        for(int i = 0; i < 3; i++){
            var -= 4;
            printf("Thread %d modified value %d
", *v, var);
        }
        printf("Modification done by Thread %d, value %d
", *v, var);
    }
    else{
        printf("Entered in Thread %d...
", *v);
        for(int i = 0; i < 3; i++){
            var *= 2;
            printf("Thread %d modified value %d
", *v, var);
        }
        printf("Modification done by Thread %d, value %d
", *v, var);
    }
}
```\n