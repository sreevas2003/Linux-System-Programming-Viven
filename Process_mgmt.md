## 3. Write a C program to demonstrate the use of fork() system call. 
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
void main()
{
        int id;
        id=fork();
        if(id==0)
        {
                int i;
                for(i=0;i<5;i++)
                {
                        printf("child process\n");
                        sleep(1);
                }
        }
        else
        {
                int i;
                for(i=0;i<5;i++)
                {
                        printf("Parent process\n");
                        sleep(1);
                }
        }
}
```
## 
