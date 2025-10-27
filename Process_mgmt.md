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
## 19. Write a program in C to create a zombie process and explain how to avoid it. 
```c
/*A zombie process is a process that has completed its execution but still has an entry in the process table to allow its parent process to read its exit status. It's "dead" but hasn't been "reaped."*/

#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
void main()
{
        int id,status;
        id=fork();
        if(id==0)
        {
                printf("Child process\n");
                exit(0);
        }
        else
        {
                printf("Parent process is sleeping for 20 sec\n");
                sleep(20);
                printf("parent sleeping completed\n");
        }
}
```

