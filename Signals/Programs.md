## 1. Write a C program to catch and handle the SIGINT signal.
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
void signal_handler(int sig) {
    printf("\nCaught SIGINT (signal number: %d)\n", sig);
    printf("We pressed Ctrl + C, but I'm not exiting!\n");
    sleep(10);
}
int main() {
    signal(SIGINT,signal_handler);
    printf("Program running... Press Ctrl + C to trigger SIGINT\n");
    while (1) {
        printf("Working...\n");
        sleep(1);
    }
    return 0;
}
```
## 2. Implement a C program to send a custom signal to another process.
```c
```
## 3. Create a C program to ignore the SIGCHLD signal temporarily.
```c
#include<stdio.h>
#include<unistd.h>
#include<signal.h>
#include<stdlib.h>
#include<sys/wait.h>
void main()
{
        struct sigaction old,new;
        new.sa_handler=SIG_IGN;
        new.sa_flags=0;
        sigemptyset(&new.sa_mask);
        printf("Temperorly Ignoring sigchld\n");
        if(sigaction(SIGCHLD,&new,&old)==-1)
        {
                perror("details:");
                exit(1);
        }
        pid_t pid;
        pid=fork();
        if(pid==0)
        {
                printf("child Process is Executed and (PID : %d)\n",getpid());
                sleep(1);
                printf("child exited\n");
                exit(0);
        }
        else
        {
                printf("parent process is Executed\n");
                sleep(2);
        }
        printf("Restoring the SIGCHLD behaviour\n");
        if(sigaction(SIGCHLD,&old,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        int status;
        waitpid(pid,&status,0);
        printf("child exit normally\n");
}
```
## 4. Write a program to block the SIGTERM signal using sigprocmask(). 
```c

