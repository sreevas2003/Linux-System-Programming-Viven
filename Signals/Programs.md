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
#### receiver.c
```c
// receiver.c — waits for a custom signal (SIGUSR1 or SIGUSR2)
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
void handle_sigusr1(int sig) {
    printf("\nReceived SIGUSR1[%d] signal from another process!\n",sig);
}
void handle_sigusr2(int sig) {
    printf("\nReceived SIGUSR2[%d] signal from another process!\n",sig);
}
int main() {
    signal(SIGUSR1, handle_sigusr1);
    signal(SIGUSR2, handle_sigusr2);
    printf("Receiver running... PID = %d\n", getpid());
    printf("Waiting for a custom signal (SIGUSR1 or SIGUSR2)...\n");
    while (1) {
        printf("Running..\n");
        sleep(1);
    }
    return 0;
}
```
#### sender.c
```c
// sender.c — sends a custom signal to another process
#include <stdio.h>
#include <signal.h>
#include <stdlib.h>
int main() {
    pid_t pid;
    int sig_choice;
    printf("Enter the PID of receiver process: ");
    scanf("%d", &pid);
    printf("Choose signal to send:\n");
    printf("1. SIGUSR1\n");
    printf("2. SIGUSR2\n");
    printf("Enter choice: ");
    scanf("%d", &sig_choice);
    int sig = (sig_choice == 1) ? SIGUSR1 : SIGUSR2;
    if (kill(pid, sig) == 0)
        printf("Sent signal %d successfully to process %d\n", sig, pid);
    else
        perror("Error sending signal");
    return 0;
}
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
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
#include<string.h>
void handler(int sig)
{
        printf("Received..the [%s] signal \n",strsignal(sig));
        exit(1);
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
        if(sigaction(SIGBUS,&act,0)==-1)
        {
                perror("DEtails:");
                exit(1);
        }
        printf("SIGSEGV and SEGBUS signals handled\n");
        raise(SIGSEGV);
        printf("Fault not occured\n");
}
```
## 24. Implement a program to handle the SIGUSR1_SIGUSR2 signal (user-defined signal). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler(int sig)
{
        if(sig==SIGUSR1)
                printf("Received the SIGUSR1 signal %d\n",sig);
        else if(sig==SIGUSR2)
                printf("received the SIGUSR2 signal %d\n",sig);
}
void main()
{
        signal(SIGUSR1,handler);
        signal(SIGUSR2,handler);
        printf("Process ID : %d\n",getpid());
        printf("send the kill -USR1 %d or kill -USR2 %d\n",getpid(),getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 25.Create a C program to handle the SIGCONT_SIGSTOP signal (continue or stop executing). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
static int cont_count=0;
static int tstp_count=0;
void handler(int sig)
{
        if(sig==SIGCONT)
        {
                cont_count++;
                printf("Received the SIGCONT signal %d\n",sig);
        }
        else if(sig==SIGTSTP)
        {
                tstp_count++;
                printf("Received the SIGTSTP signal %d\n",sig);
        }
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        if(sigaction(SIGCONT,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        if(sigaction(SIGTSTP,&act,0)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("SIGCONT and SIGTSTP signals are installed\n");
        printf("send kill -TSTP %d and kill -CONT %d\n",getpid(),getpid());
        int i;
        for(i=0;;i++)
        {
                printf("%d ticks, %d cont count and %d tstp count\n",i,cont_count,tstp_count);
                sleep(1);
        }
}
```
## 26.Write a program to demonstrate how to block signals using sigprocmask(). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handler(int sig)
{
        printf("Received the signal %d\n",sig);
}
void main()
{
        sigset_t set,old;
        signal(SIGINT,handler);
        sigemptyset(&set);
        sigaddset(&set,SIGINT);
        printf("Block the SIGINT signal for 10 sec..\n");
        sigprocmask(SIG_BLOCK,&set,&old);
        sleep(10);
        printf("now unblock the SIGINT signal :\n");
        sigprocmask(SIG_SETMASK,&old,NULL);
        printf("send the signal kill -2 %d\n",getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 27.Write a program to implement a timer using signals.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
#include<sys/time.h>
static int ticks=0;
void handler(int sig)
{
        ticks++;
        printf("Received timer : %d ticks\n",ticks);
}
void main()
{
        signal(SIGALRM,handler);
        struct itimerval timer;
        timer.it_value.tv_sec=1;
        timer.it_value.tv_usec=0;
        timer.it_interval.tv_sec=1;
        timer.it_interval.tv_usec=0;
        setitimer(ITIMER_REAL,&timer,0);
        while(ticks<10)
                pause();
        timer.it_value.tv_sec=0;
        timer.it_value.tv_usec=0;
        timer.it_interval.tv_sec=0;
        timer.it_interval.tv_usec=0;
        setitimer(ITIMER_REAL,&timer,0);
        printf("timer reached Max ticks and exited\n");
}
```
## 28.Write a program to handle a real-time signal using sigqueue(). 
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdlib.h>
void handler(int sig, siginfo_t *info, void *context)
{
    printf("Received signal %d with value %d\n", sig, info->si_value.sival_int);
}
int main()
{
    pid_t pid = getpid();
    struct sigaction sa;
    union sigval value;
    sa.sa_sigaction = handler;
    sa.sa_flags = SA_SIGINFO;
    sigemptyset(&sa.sa_mask);
    sigaction(SIGRTMIN, &sa, NULL);
    printf("My PID = %d\n", pid);
    printf("Sending SIGRTMIN to myself using sigqueue...\n");
    value.sival_int = 1234;
    sigqueue(pid, SIGRTMIN, value);
    sleep(1);
    return 0;
}
```
## 29.Write a program to handle SIGALRM (alarm clock) signal for implementing a timeout mechanism in system programming.
```c
#include <unistd.h>
#include <signal.h>
#include <string.h>
#include <errno.h>
#define TIMEOUT 5
#define BUF_SIZE 256
volatile sig_atomic_t timed_out = 0;
void alarm_handler(int sig)
{
    (void)sig;
    timed_out = 1;
    const char msg[] = "\nTimeout! (SIGALRM)\n";
    write(STDOUT_FILENO, msg, sizeof(msg) - 1);
}
int main(void)
{
    struct sigaction sa;
    char buf[BUF_SIZE];
    ssize_t n;
    sa.sa_handler = alarm_handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;
    if (sigaction(SIGALRM, &sa, NULL) == -1) {
        perror("sigaction");
        exit(EXIT_FAILURE);
    }
    printf("You have %d seconds to type a line and press Enter:\n> ", TIMEOUT);
    fflush(stdout);
    alarm(TIMEOUT);
    n = read(STDIN_FILENO, buf, BUF_SIZE - 1);
    if (n < 0) {
        if (errno == EINTR && timed_out) {
            printf("No input received within %d seconds. Exiting.\n", TIMEOUT);
            return 0;
        } else {
            perror("read");
            return 1;
        }
    }
    alarm(0);
    buf[n] = '\0';
    printf("You typed: %s", buf);
    return 0;
}
```
## 30. Create a C program to handle the SIGTRAP signal (trace/breakpoint trap). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handler(int sig)
{
        printf("Received the signal %d\n",sig);
        sleep(1);
}
void main()
{
        signal(SIGTRAP,handler);
        printf("SIGTRAP signal is installed\n");
        printf("raise sigtrap signal or kill -TRAP %d\n",getpid());
        raise(SIGTRAP);
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 31. Create a C program to handle the SIGWINCH_SIGINFO signal (window size change or status request from keyboard). 
```c
// Handle SIGWINCH and SIGINFO signals in C
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
void handle_sigwinch(int sig) {
    printf("\nReceived SIGWINCH (window size change) signal!\n");
}
void handle_siginfo(int sig) {
    printf("\nReceived SIGINFO (status request) signal!\n");
}
int main() {
    signal(SIGWINCH, handle_sigwinch);
#ifdef SIGINFO
    signal(SIGINFO, handle_siginfo);
#endif
    printf("Program running... PID: %d\n", getpid());
    printf("Resize the terminal or send a signal using:\n");
    printf("  kill -WINCH %d\n", getpid());
#ifdef SIGINFO
    printf("  kill -INFO %d\n", getpid());
#endif
    while (1) {
        printf("Runnig..\n");
        sleep(1);
    }

    return 0;
}
```
## 32. Implement a program to handle the SIGSYS_SIGPIPE signal (bad system call or write on a pipe with no one to read it). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler_pipe(int sig)
{
        printf("Received the SIGPIPE signal %d\n",sig);
}
void handler_sys(int sig)
{
        printf("Received the SIGSYS signal %d\n",sig);
}
void main()
{
        signal(SIGPIPE,handler_pipe);
        signal(SIGSYS,handler_sys);
        printf("SIGSYS handler is installed \n");
        int fd[2];
        pipe(fd);
        if(fork()==0)
        {
                close(fd[1]);
                close(fd[0]);
                exit(0);
        }
        else
        {
                close(fd[0]);
                sleep(1);
                write(fd[1],"Hello",5);
                raise(SIGSYS);
                close(fd[1]);
        }
        printf("Program finished\n");
}
```
## 33. Write a program to handle the SIGURG_SIGTSTP signal (urgent condition on socket or stop signal). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handler_tstp(int sig)
{
        printf("Received the signal %d\n",sig);
        sleep(10);
}
void handler_urg(int sig)
{
        printf("Received the signal %d\n",sig);
        sleep(10);
}
void main()
{
        signal(SIGTSTP,handler_tstp);
        signal(SIGURG,handler_urg);
        printf("Both signals are installed\n");
        printf("to SIGSTSTP trigger then press cntl+Z \n");
        printf("for SIGURG to send manually kill -URG %d\n",getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 34. Create a C program to handle the SIGCONT_SIGSTOP signal (continue or stop executing). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handler_cont(int sig)
{
        printf("Received the SIGCONT signal %d\n",sig);
        sleep(2);
}
void handler_stop(int sig)
{
        printf("Received the SIGSTOP signal %d\n",sig);
        sleep(2);
}
void main()
{
        signal(SIGSTOP,handler_stop);
        signal(SIGCONT,handler_cont);
        printf("Both signal are installed\n");
        printf(" Manually send kill -CONT %d and kill -STOP %d\n",getpid(),getpid());
        while(1)
        {
                printf("Runnig..\n");
                sleep(1);
        }
}
```
## 35. Implement a program to handle the SIGTTIN_SIGTTOU signal (background read or write attempted from control terminal). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handler_in(int sig)
{
        printf("Received the TTIN signal %d\n",sig);
        sleep(1);
}
void handler_out(int sig)
{
        printf("Received the TTOU signal %d\n",sig);
        sleep(1);
}
void main()
{
        signal(SIGTTIN,handler_in);
        signal(SIGTTOU,handler_out);
        printf("Both signals are installed\n");
        printf("send kill -TTIN %d and kill -TTOU %d\n",getpid(),getpid());
        while(1)
        {
                printf("Runnig..\n");
                sleep(1);
        }
}
```
## 36. Create a C program to handle the SIGXCPU_SIGXFSZ signal (CPU time limit exceeded or file size limit exceeded).
```c
#include<stdio.h>
#include<sys/resource.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void handler_cpu(int sig)
{
        printf("Received the SIGXCPU signal %d\n",sig);
        sleep(1);
}
void handler_fsz(int sig)
{
        printf("Received the SIGXESZ signal %d\n",sig);
        sleep(1);
}
void main()
{
        struct rlimit limit;
        limit.rlim_cur=2;
        limit.rlim_max=3;
        setrlimit(RLIMIT_CPU,&limit);
        limit.rlim_cur=1024;
        limit.rlim_max=4096;
        setrlimit(RLIMIT_FSIZE,&limit);
        signal(SIGXCPU,handler_cpu);
        signal(SIGXFSZ,handler_fsz);
        printf("Both signals are installed\n");
        printf("trigger kill -SIGXCPU %d\n",getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 37. Implement a program to handle the SIGVTALRM_SIGWINCH signal (virtual timer expired or window size change). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler_vt(int sig)
{
        printf("Received the signal %d\n",sig);
}
void handler_win(int sig)
{
        printf("Received the signal %d\n",sig);
}
void main()
{
        signal(SIGVTALRM,handler_vt);
        signal(SIGWINCH,handler_win);
        printf("Both signals are installed\n");
        printf("Send kill -VTALRM %d or kill -WINCH %d\n",getpid(),getpid());
        while(1)
        {
                printf("Running..\n");
                sleep(1);
        }
}
```
## 38. Write a program on watch dog timer and it explain its working 
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler(int sig)
{
        printf("watchdog timer expired!program is not responsed\n");
}
void main()
{
        signal(SIGALRM,handler);
        alarm(5);
        printf("Program started. ALArm set for 5 seconds\n");
        int i;
        for(i=0;i<=3;i++)
        {
                printf("Working ...reset watchdog(%d)\n",i);
                sleep(1);
                alarm(5);
        }
        printf("watchdog will expire now\n");
        while(1)
        {
                printf("Infinite loop\n");
                sleep(1);
        }
}
```
## 39. Write a program to demonstrate how to handle and recover from a segmentation fault (SIGSEGV) in a system programming scenario.
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void handler(int sig)
{
        printf("Received the signal %d\n",sig);
        printf("Now we can safely exit\n");
        exit(0);
}
void main()
{
        struct sigaction act;
        act.sa_handler=handler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        sigaction(SIGSEGV,&act,0);
        printf("SIGSEGV signal is installed now raise seg fault\n");
        raise(SIGSEGV);
        while(1)
        {
                printf("Running...\n");
                sleep(1);
        }
        printf("This line not showing\n");
}
```
## 40. Write a program to demonstrate how to block signals using sigprocmask(). 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void main()
{
        sigset_t new,old;
        sigemptyset(&new);
        sigaddset(&new,SIGINT);
        sigprocmask(SIG_BLOCK,&new,&old);
        int i=0;
        while(i<10)
        {
                printf("Running...%d\n",i);
                i++;
                sleep(1);
        }
        sigprocmask(SIG_UNBLOCK,&old,NULL);
        printf("sigint is unblocked\n");
}
```
## 
