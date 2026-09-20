---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems on Process Synchronization

Code analysis practice problems exploring thread concurrency, mutex locks (pthread_mutex_t), and semaphores (sem_t) in C.
Docx last modified: 07-11-2025.

## Content

### Problem 1: Mutex Lock and Binary Semaphore Synchronization
Analysis of two threads sharing a global integer `sum = 15`, synchronized using semaphore `s` (initialized to `0`) and mutex `m`. Thread 1 waits for `s`, while Thread 2 executes first, multiplies `sum` by 3 five times, and signals `s`.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>
#include <semaphore.h>

int t_id[] = {1, 2};
void *t_func1(int *id);
void *t_func2(int *id);
int sum = 15;
pthread_mutex_t m;
sem_t s;

int main(){
    pthread_t t[2];
    sem_init(&s, 0, 0);
    pthread_mutex_init(&m, NULL);
    pthread_create(&t[0], NULL, (void *)t_func1, &t_id[0]);
    pthread_create(&t[1], NULL, (void *)t_func2, &t_id[1]);
    for(int i = 0; i < 2; i++){
        pthread_join(t[i], NULL);
    }
    sem_destroy(&s);
    pthread_mutex_destroy(&m);
    printf("Total sum: %d
", sum);
    return 0;
}

void *t_func1(int *id){
    sem_wait(&s);
    pthread_mutex_lock(&m);
    for(int i = 0; i < 5; i++){
        printf("Sum: %d
", sum);
        sum -= 10;
    }
    pthread_mutex_unlock(&m);
    sem_post(&s);
}

void *t_func2(int *id){
    pthread_mutex_lock(&m);
    for(int i = 0; i < 5; i++){
        printf("Sum: %d
", sum);
        sum *= 3;
    }
    pthread_mutex_unlock(&m);
    sem_post(&s);
}
```

---

### Problem 2: Dual Semaphore Signaling (`s1`, `s2`)
Two threads operating on global integer `sum = 0`. Semaphore `s1` is initialized to `1`, allowing `t_func1` to execute first (adding 10 ten times) and then posting both `s1` and `s2` to activate `t_func2` (subtracting 5 ten times).

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>
#include <semaphore.h>

int t_id[] = {1, 2};
void *t_func1(int *id);
void *t_func2(int *id);
int sum = 0;
sem_t s1, s2;

int main(){
    pthread_t t[2];
    sem_init(&s1, 0, 1);
    sem_init(&s2, 0, 0);
    pthread_create(&t[0], NULL, (void *)t_func1, &t_id[0]);
    pthread_create(&t[1], NULL, (void *)t_func2, &t_id[1]);
    for(int i = 0; i < 2; i++){
        pthread_join(t[i], NULL);
    }
    sem_destroy(&s1);
    sem_destroy(&s2);
    printf("Total sum: %d
", sum);
    return 0;
}

void *t_func1(int *id){
    sem_wait(&s1);
    for(int i = 0; i < 10; i++){
        printf("Sum: %d
", sum);
        sum += 10;
    }
    sem_post(&s1);
    sem_post(&s2);
}

void *t_func2(int *id){
    sem_wait(&s2);
    for(int i = 0; i < 10; i++){
        printf("Sum: %d
", sum);
        sum -= 5;
    }
    sem_post(&s2);
}
```

---

### Problem 3: Three-Thread Chained Pipeline Synchronization (`s1`, `s2`, `s3`)
Three threads with `sum = 13` initialized with semaphores `s1 = 0`, `s2 = 1`, `s3 = 0`. Execution order is strictly orchestrated: Thread 2 runs first ($sum + 7$ five times $\implies 48$) and posts `s3`; Thread 3 runs second ($sum - 3$ five times $\implies 33$) and posts `s1`; Thread 1 runs third ($sum \times 2$ five times $\implies 1056$).

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>
#include <semaphore.h>

int t_id[] = {1, 2, 3};
void *t_func1(int *id);
void *t_func2(int *id);
void *t_func3(int *id);
int sum = 13;
sem_t s1, s2, s3;

int main(){
    pthread_t t[3];
    sem_init(&s1, 0, 0);
    sem_init(&s2, 0, 1);
    sem_init(&s3, 0, 0);
    pthread_create(&t[0], NULL, (void *)t_func1, &t_id[0]);
    pthread_create(&t[1], NULL, (void *)t_func2, &t_id[1]);
    pthread_create(&t[2], NULL, (void *)t_func3, &t_id[2]);
    for(int i = 0; i < 3; i++){
        pthread_join(t[i], NULL);
    }
    sem_destroy(&s1);
    sem_destroy(&s2);
    sem_destroy(&s3);
    printf("Total sum: %d
", sum);
    return 0;
}

void *t_func1(int *id){
    sem_wait(&s1);
    for(int i = 0; i < 5; i++){
        printf("Sum: %d
", sum);
        sum *= 2;
    }
    sem_post(&s1);
}

void *t_func2(int *id){
    sem_wait(&s2);
    for(int i = 0; i < 5; i++){
        printf("Sum: %d
", sum);
        sum += 7;
    }
    sem_post(&s2);
    sem_post(&s3);
}

void *t_func3(int *id){
    sem_wait(&s3);
    for(int i = 0; i < 5; i++){
        printf("Sum: %d
", sum);
        sum -= 3;
    }
    sem_post(&s3);
    sem_post(&s1);
}
```\n