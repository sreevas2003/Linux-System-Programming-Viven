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
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<signal.h>
#include<stdlib.h>
void handler(int sig)
{
        printf("caught %s signal and number is %d\n",strsignal(sig),sig);
        sleep(2);
}
void main()
{
        sigset_t set;
        signal(SIGTERM,handler);
        sigemptyset(&set);
        sigaddset(&set,SIGTERM);
        printf("SIGTERM is trying to Block\n");
        if(sigprocmask(SIG_BLOCK,&set,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("SIGTERM is now Blocked\n");
        printf("Now try sendig (kill -TERM %d)\n",getpid());
        printf("sleeping 10 seconds\n");
        sleep(15);
        printf("SIGTERM is trying to Un-Block\n");
        if(sigprocmask(SIG_UNBLOCK,&set,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("SIGTERM is now unblocked\n");
        printf("Now try sendig (kill -TERM %d)\n",getpid());
        while(1)
                pause();
}
```
## 5. Implement a C program to handle the SIGALRM signal using sigaction(). 
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>
static int got=0;
void handler(int sig)
{
        printf("Cought the %s and number is %d\n",strsignal(sig),sig);
        got=1;
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGALRM,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("Set alarm for 3 seconds\n");
        alarm(3);
        while(!got)
                pause();
        printf("Getting alarm from alarm handler\n");
}
```
## 6. Write a C program to install a custom signal handler for SIGTERM? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
#include<string.h>
void handler(int sig)
{
        printf("Caught %s signal and number is %d\n",strsignal(sig),sig);
        sleep(2);
}
void main()
{
        if(signal(SIGTERM,handler)==SIG_ERR)
        {
                perror("Details:");
                exit(1);
        }
        printf("Custom signal is installed,\n");
        printf("Now we try to send kill -15 %d\n",getpid());
        while(1)
        {
                printf("Running....waiting for SIGTERM\n");
                sleep(1);
        }
}
```
## 7. Implement a program to handle the SIGSEGV signal (segmentation fault). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<signal.h>
#include<unistd.h>
void handler(int sig)
{
        printf("Signal is received %d",sig);
        char msg[]="SIGSEGV is received and Exited\n";
        write(STDERR_FILENO,msg,sizeof(msg)-1);
        _Exit(128+SIGSEGV);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGSEGV,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }

         *(volatile int *)0 = 42;
}
```
## 8. Create a program to handle the SIGILL signal (illegal instruction). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
#include<string.h>
void handler(int sig)
{
        printf("Caught the [%s] signal and number is [%d]\n",strsignal(sig),sig);
        _Exit(128 + SIGILL);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGILL,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        raise(SIGILL);
}
```
## 9. Write a program to handle the SIGABRT signal (abort).
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
#include<string.h>
void handler(int sig)
{
        printf("caught [%s]signal and number is %d\n",strsignal(sig),sig);
        _Exit(128 + SIGABRT);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGABRT,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        abort();
}
```
## 10. Implement a C program to handle the SIGQUIT signal. 
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>
#include<signal.h>
void handler(int sig)
{
        printf("Caught [%s] signal and number is %d\n",strsignal(sig),sig);
        _Exit(128+SIGQUIT);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGQUIT,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("Custom signal installed now enter Cntl+\\ for quit\n");
        while(1)
        {
                printf("Running\n");
                sleep(1);
        }
}
```
## 11. Write a program to handle the SIGTERM signal (termination request). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<signal.h>
void handler(int sig)
{
        printf("Caught the [%s] signal and number is %d\n",strsignal(sig),sig);
        sleep(5);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGTERM,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("Custom signal is installed\nNow send kill -15 %d\n",getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 12. Write a program to handle the SIGTSTP signal (terminal stop). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<signal.h>
void handler(int sig)
{
        printf("Caught the [%s] signal and number is %d\n",strsignal(sig),sig);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGTSTP,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("custom signal is Blocked\n");
        while(1)
        {
                printf("Running..waiting for termination\n");
                sleep(1);
        }
}
```
## 13. Write a program to handle the SIGVTALRM signal (virtual timer expired). 
```c
#include<stdio.h>
#include<sys/time.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<signal.h>
void handler(int sig)
{
        printf("Caught the[%s] signal and nuber is %d\n",strsignal(sig),sig);
        sleep(3);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGVTALRM,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        struct itimerval timer;
        timer.it_value.tv_sec=1;
        timer.it_value.tv_usec=0;
        timer.it_interval.tv_sec=1;
        timer.it_interval.tv_usec=0;
        setitimer(ITIMER_VIRTUAL,&timer,0);
        printf("Virtual timer started.Waiting for signals\n");
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 14. Write a program to handle the SIGWINCH signal (window size change). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
#include<string.h>
#include<sys/ioctl.h>
static int got=0;
void handler(int sig)
{
        printf("Caught the [%s] signal and number is %d\n",strsignal(sig),sig);
        got=1;
        sleep(2);
}
void window(void)
{
        struct winsize ws;
        if(ioctl(STDOUT_FILENO,TIOCGWINSZ,&ws)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("Window size: %d rows and %d columns\n",(int)ws.ws_row,(int)ws.ws_col);
        fflush(stdout);
}

void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);

        if(sigaction(SIGWINCH,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }

        printf("SIGWINCH handler is installed. \n");
        window();
        printf("Press Cntl+C to exit\n");
        while(1)
        {
                printf("Runnig..\n");
                sleep(1);
                if(got)
                {
                        got=0;
                        window();
                }
        }
}
```
## 15. Implement a C program to handle the SIGXFSZ signal (file size limit exceeded). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/resource.h>
#include<signal.h>
void handler(int sig)
{
        printf("SIGXFSZ signal is received file size limit exceeded\n");
        exit(1);
}
void main()
{
        signal(SIGXFSZ,handler);
        struct rlimit limit;
        limit.rlim_cur=2048;
        limit.rlim_max=2048;
        setrlimit(RLIMIT_FSIZE,&limit);
        int fd;
        char data[1024]="This is a test line to exceed file size limit.\n";
        fd=open("output.txt",O_RDWR | O_CREAT | O_TRUNC,0644);
        printf("Writing a data until file size exceede\n");
        while(1)
        {
                write(fd,data,sizeof(data));
                sleep(1);
        }
        close(fd);
}
```
## 16. Create a program to handle the SIGPWR signal (power failure restart).
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<signal.h>
void handler(int sig)
{
        printf("Received [%s] signal Cleaning up and exiting\n",strsignal(sig));
        exit(0);
}
void main()
{
        signal(SIGPWR,handler);
        printf("SIGPWR signal is installed\n");
        printf("Sleep for 3 seconds\n");
        sleep(3);
        printf("Now raising the siganl\n");
        raise(SIGPWR);
        while(1)
                pause();
}
```
## 17. Write a program to handle the SIGSYS signal (bad system call). 
```c
include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handler(int sig)
{
        printf("Caught [%s] signal is received\n",strsignal(sig));
        sleep(1);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGSYS,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("SIGSYS signal is installed..PID is %d\n",getpid());
        printf("Sending the signal kill -SIGSYS %d\n",getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 18. Write a C program to handle the SIGIO signal (I/O is possible on a descriptor). 
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handler(int sig)
{
        printf("Caught the signal %d: %s\n",sig,strsignal(sig));
        sleep(5);
}
void main()
{
        signal(SIGIO,handler);
        printf("SIGIO signal is installed..\n");
        printf("Raising SIGIO ..\n");
        raise(SIGIO);
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 19. Implement a C program to handle the SIGINFO signal (status request from keyboard). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<signal.h>
void handler(int sig)
{
        printf("Caught the signal %d\n",sig);
}
void main()
{
#ifdef SIGINFO
        signal(SIGINFO,handler);
        printf("Waiting for SIGINFO signal Press Cntl+T\n");
#else
        signal(SIGUSR1,handler);
        printf("Waiting for SIgUSR signal press kill -USR1 %d\n",getpid());
#endif
        while(1)
        {
                printf("Runnig...\n");
                sleep(1);
        }
}
```
## 20. Create a C program to handle the SIGRTMIN signal (minimum real-time signal).
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler(int sig)
{
        printf("Received the signal %d\n",sig);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGRTMIN,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("SIGRTMIN signal is arrived\n");
        printf("Send kill -SIGRTMIN %d\n",getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 21. Implement a program to handle the SIGRTMAX signal (maximum real-time signal). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler(int sig)
{
        printf("Received the signal %d\n",sig);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGRTMAX,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("SIGRTMAX signal is installed..\n");
        printf("Send kill -SIGRTMAX %d\n",getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 22. Write a program to handle the SIGABRT_ABORT signal (abort signal). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler(int sig)
{
        printf("Received the signal %d\n",sig);

}
void main()
{
        signal(SIGABRT,handler);
        printf("Send abort signal \n");
        abort();
        printf("This is not shown after abort signal and exited\n");
}
```
## 23. Create a C program to handle the SIGSEGV_SIGBUS signal (segmentation fault or bus error). 
```c

```
