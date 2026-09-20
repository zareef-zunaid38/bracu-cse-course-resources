---
source: original file no longer available
fidelity: the original file is no longer available, so this text version is the only record; it has not been re-verified against the original
generated: earlier (2026-09); header added 2026-09-20
---
# Practice Problems on Fork()

CSE321 practice problems on C's `fork()` system call, predicting outputs of various process-spawning code blocks.
Docx last modified: 06-10-2025.

## Content

### Problem 1
Find outputs of the following code:
```c
main(){
	fork();
	fork();
	printf("hi\n");
	fork();
	printf("hello\n");
	fork();
	printf("bye\n");
}
```

### Problem 2
Find outputs of the following code:
```c
main(){
	fork();
	fork();
	c=fork();
	if(c>0){
		printf("hi\n");
		fork();
	}
	fork();
	printf("bye\n");
}
```

### Problem 3
Find outputs of the following code:
```c
int main(){
	pid_t p;
	int a=3;
	int b=11;
	char s[20];
	p=fork();
	if(p<0){
		printf("fork failed\n");
	}
	else if(p==0){
		strcpy(s,"child");
		a=a*b;
		b=b/a;
	}
	else{
		wait();
		strcpy(s,"parent");
		a=a+b;
		b=b-a;
	}
	printf("%s is printing a= %d\n",s,a);
	printf("%s is printing b= %d\n",s,b);
	
	return 0;
}
```

### Problem 4
Find outputs of the following code:
```c
static int a=5;
static int b=3;
int main(){
	pid_t x, y;
	x=fork();
	if(x<0){
		printf("fork failed\n");
	}
	else if(x>0){
		a=a+5;
		b=b-5;
		wait();
		y=fork();
		if(y<0){
			printf("fork failed\n");
		}
		else if(y>0){
			wait();
			a=a-2;
			b=b+2;
		}
		else{
			a=a*2;
			b=b/3;
		}
	}
	else{
		a=a/2;
		b=b*3;
	}
	printf("a= %d\n",a);
	printf("b= %d\n",b);
	
	return 0;
}
```
