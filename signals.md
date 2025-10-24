## 1. Write a C program to catch and handle the SIGINT signal.
```c
#include<stdio.h>
#include<signal.h>
#include<stdlib.h>
mysignal(int signo)
{
        printf("My signal Handler is %d\n",signo);
        sleep(10);
}
void main()
{
        signal(SIGINT,mysignal);
        while(1)
        {
                printf("hey\n");
                sleep(1);
        }
}
```
## 2. Implement a C program to send a custom signal to another process.
```c
```
## 3. Create a C program to ignore the SIGCHLD signal temporarily.
