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
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
void main()
{
        int i,n,pid;
        printf("No.of childs\n");
        scanf("%d",&n);
        for(i=1;i<=n;i++)
        {
                pid=fork();
                if(pid==0)
                {
                        printf("%d child is pid is %d and ppid is %d\n",i,getpid(),getppid());
                        exit(0);
                }
        }
        for(i=1;i<=n;i++)
                wait(NULL);
        printf("Parent pid %d\n",getpid());
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
## 22. Write a C program to demonstrate the use of the waitpid() function for process synchronization. 
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/wait.h>
#include<unistd.h>
void main()
{
        int sta1,sta2,pid1,pid2;
        pid1=fork();
        if(pid1==0)
        {
                printf("Child 1 process created %d\n",getpid());
                sleep(10);
                exit(0);
        }
        pid2=fork();
        if(pid2==0)
        {
                printf("Child 1 process created %d\n",getpid());
                sleep(10);
                exit(0);
        }
        waitpid(pid1,&sta1,0);
        waitpid(pid2,&sta2,0);
        printf("%d %d",WEXITSTATUS(sta1),WEXITSTATUS(sta2));
}
```
## 28 Write a C program using system() to execute shell commands
```c
#include<stdio.h>
#include<stdlib.h>
void main()
{
        char str[50];
        printf("enter any command : ");
        gets(str);
        int res=system(str);
        if(res==-1)
                printf("Error Occured\n");
        else
                printf("%s command executed successfully and %d\n",str,WEXITSTATUS(res));
}
```
## 
