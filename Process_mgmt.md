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
## 6. Write a C program to illustrate the use of the execvp() function.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
void main()
{
        char* argv[5];
        argv[0]="ls";
        argv[1]="-l";
        argv[2]='\0';

        execvp("ls",argv);
}
```
## 10. Write a program in C to create a child process using fork() and print its PID. 
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<sys/types.h>
void main()
{
        int id=fork();
        if(id==0)
        {
                printf("Child process PID is %d and PPID is %d\n",getpid(),getppid());
                sleep(5);
        }
        else{
                printf("Parent process PID is %d and PPID is %d\n",getpid(),getppid());
                sleep(5);
        }
}
```
## 13. Explain how the execv() system call works and provide a code example.
```c
#include<stdio.h>
#include<stdlib.h>
void main()
{
        char *argv[5];
        argv[0]="ps";
        argv[1]="-ef";
        argv[2]='\0';
        execv("/bin/ps",argv);
}
```
## 15. Write a C program to create multiple child processes using fork() and display their PIDs.
```c

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

