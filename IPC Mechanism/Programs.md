## 1.Implement a program that uses pipes for communication between a parent and child process. Show how data can be passed between processes using pipes.
```c
#include<stdio.h>
#include<unistd.h>
#include<string.h>
int main()
{
        int fd[2];
        pipe(fd);
        int pid=fork();
        if(pid==0)
        {
                char buf[20];
                read(fd[0],buf,sizeof(buf));
                printf("Child got:%s",buf);
                printf("\n");
        }
        else
        {
                char text[]="Hello from parent";
                write(fd[1],text,strlen(text)+1);
        }
        return 0;
}
```
## 2.Create a program where two processes communicate synchronously using pipes. Ensure that one process waits for the other to finish before proceeding.
```c
#include<stdio.h>
#include<unistd.h>
#include<string.h>
int main()
{
        int p1[2],p2[2];
        pipe(p1);
        pipe(p2);
        if(fork()==0)
        {
                char buf[50];
                read(p1[0],buf,sizeof(buf));
                printf("child got:%s\n",buf);
                write(p2[1],"ACK",4);
        }
        else
        {
                write(p1[1],"Hello",6);
                char ack[10];
                read(p2[0],ack,sizeof(ack));
                printf("parent got:%s\n",ack);
        }
        return 0;
}
```
## 3.Implement a program that uses Named pipes for communication between two processes.
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/stat.h>
#include<string.h>
int main()
{
        char *fifo="/tmp/myfifo";
        mkfifo(fifo,0666);
        if(fork()==0)
        {
                int fd=open(fifo,O_WRONLY);
                write(fd,"Hello FIFO",11);
                close(fd);
        }
        else
        {
                char buf[50];
                int fd=open(fifo,O_RDONLY);
                read(fd,buf,sizeof(buf));
                printf("Reader got:%s\n",buf);
                close(fd);
        }
        return 0;
}
```
## 4.Write a C program to create a message queue using the msgget system call. Ensure that the program checks for errors during the creation process.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/msg.h>
int main()
{
        key_t key=ftok(".",'A');
        int id=msgget(key,IPC_CREAT|0666);
        if(id==-1)
                perror("msgget");
        else
                printf("Message Queue created with ID:%d\n",id);
        return 0;
}
```

## 5.Develop two separate C programs, one for sending messages and the other for receiving messages through a created message queue.
### Sender:
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#include<string.h>
struct msg
{
        long type;
        char text[50];
};
int main()
{
        key_t key=ftok(".",'A');
        int id=msgget(key,IPC_CREAT|0666);
        struct msg m={1,"Hello from sender"};
        msgsnd(id,&m,sizeof(m.text),0);
        return 0;
}
```
### Receiver:
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/msg.h>
struct msg
{
        long type;
        char text[50];
};
int main()
{
        key_t key=ftok(".",'A');
        int id=msgget(key,0666);
        struct msg m;
        msgrcv(id,&m,sizeof(m.text),1,0);
        printf("Receieved:%s\n",m.text);
        return 0;
}
```
## 6.Create a program to remove an existing message queue using the msgctl system call.Ensure that the program prompts the user for confirmation before deleting the message queue.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/msg.h>
int main()
{
        key_t key;
        int msqid;
        char choice;

        key=ftok(".",'A');
        if(key==-1)
        {
                perror("ftok");
                return 1;
        }

        msqid=msgget(key,0666);
        if(msqid==-1)
        {
                perror("msgget");
                return 1;
        }

        printf("Message Queue ID:%d\n",msqid);
        printf("Do you really want to delete this message queue?(y/n): ");
        scanf(" %c",&choice);

        if(choice=='y'||choice=='Y')
        {
                if(msgctl(msqid,IPC_RMID,NULL)==-1)
                {
                        perror("msgctl");
                        return 1;
                }
        printf("Message queue deleted successfully.\n");
        }
        else
        {
                printf("Deletion cancelled.\n");
        }
        return 0;
}
```
## 7.Design a multithreaded program where threads communicate through named pipes.
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<fcntl.h>
#include<unistd.h>
#include<sys/stat.h>
#include<string.h>

#define FIFO_NAME "mkfifo"

void* writer_thread(void* arg)
{
        int fd=open(FIFO_NAME,O_WRONLY);
        char msg[]="Hello from writer";

        write(fd, msg, strlen(msg)+1);
        printf("Writer:sent message\n");

        close(fd);
        return NULL;
}

void *reader_thread(void* arg)
{
        int fd=open(FIFO_NAME, O_RDONLY);
        char buffer[100];

        read(fd,buffer,sizeof(buffer));
        printf("Reader:received message:%s\n",buffer);

        close(fd);
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        mkfifo(FIFO_NAME,0666);
        pthread_create(&t1,NULL,writer_thread,NULL);
        sleep(1);
        pthread_create(&t2,NULL,reader_thread,NULL);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        unlink(FIFO_NAME);
        return 0;
}
```
## 8.Write a C program where two processes communicate using message queues.Implement sending and receiving messages between the processes using msgget,msgsnd, and msgrcv.
### sender:
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <string.h>

struct msg_buffer {
    long msg_type;
    char msg_text[100];
};

int main() {
    key_t key;
    int msgid;

    key = ftok(".", 'A');
    if (key == -1) {
        perror("ftok");
        return 1;
    }

    msgid = msgget(key, IPC_CREAT | 0666);
    if (msgid == -1) {
        perror("msgget");
        return 1;
    }

    struct msg_buffer message;
    message.msg_type = 1;
    strcpy(message.msg_text, "Hello from sender process!");

    if (msgsnd(msgid, &message, sizeof(message.msg_text), 0) == -1) {
        perror("msgsnd");
        return 1;
    }

    printf("Sender: Message sent successfully.\n");
    return 0;
}
```
### receiver:
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>

struct msg_buffer {
    long msg_type;
    char msg_text[100];
};

int main() {
    key_t key;
    int msgid;

    key = ftok(".", 'A');
    if (key == -1) {
        perror("ftok");
        return 1;
    }

    msgid = msgget(key, 0666);
    if (msgid == -1) {
        perror("msgget");
        return 1;
    }

    struct msg_buffer message;

    if (msgrcv(msgid, &message, sizeof(message.msg_text), 1, 0) == -1) {
        perror("msgrcv");
        return 1;
    }

    printf("Receiver: Message received: %s\n", message.msg_text);
    msgctl(msgid, IPC_RMID, NULL);

    return 0;
}
```
## 9.Implement a program where two processes communicate synchronously using message queues. Ensure that one process waits for the other to finish before proceeding.
### server
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/wait.h>
#include<sys/msg.h>

#define KEY 16548
#define SRV_MSG_TYPE 1

void main()
{
        int msqid;
        char rxbuf[50]={0};
        msqid=msgget(KEY,0666|IPC_CREAT);
        printf("Waiting for message from client\n");
        msgrcv(msqid,rxbuf,50,SRV_MSG_TYPE,0);
        printf("Message received from client:%s\n",rxbuf+8);
}
```
### client
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/wait.h>
#include<sys/msg.h>
#include<string.h>

#define KEY 16548

#define SRV_MSG_TYPE 1

void main()
{
        int msqid;
        char txbuf[50];
        msqid=msgget(KEY,0);
        long int *ptr=(long *)txbuf;
        ptr[0]=SRV_MSG_TYPE;
        printf("Enter the message for server:");
        scanf("%s",txbuf+8);
        msgsnd(msqid,txbuf,strlen(txbuf)+1+8+8,0);
        printf("Message sent to server\n");
}
```
## 10.Write a C program that initializes a shared memory segment using shmget.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<errno.h>

#define KEY 1234

int main()
{
        int shmid;
        shmid=shmget(KEY,1024,0666|IPC_CREAT);
        if(shmid==-1)
        {
                perror("shmget");
                return 1;
        }
        printf("shared memory created successfully with ID:%d\n",shmid);
        return 0;
}
```
## 11.Develop a program that attaches to a previously created shared memory segment using shmat and detaches using shmdt.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>

#define KEY 1234

int main()
{
        int shmid;
        char *data;

        shmid=shmget(KEY,1024,0666);
        if(shmid==-1)
        {
                perror("shmget");
                return 1;
        }

        data=(char *)shmat(shmid,NULL,0);
        if(data==(char *)-1)
        {
                perror("shmamt");
                return 1;
        }
        printf("Attached to shared memory.Current content:%s\n",data);
        printf("ENter new data:");
        scanf("%s",data);
        if(shmdt(data)==-1)
        {
                perror("shmdt");
                return 1;
        }
        printf("Detached from shared memory successfully.\n");
        return 0;
}
```
## 12.Create a program that forks multiple processes, and each process communicates using shared memory.
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<sys/wait.h>
#include<unistd.h>

#define KEY 1234

int main()
{
        int shmid;
        int *shared_data;

        shmid=shmget(KEY,4*sizeof(int),0666|IPC_CREAT);
        if(shmid==-1)
        {
                perror("shmget");
                return 1;
        }
        shared_data=(int *)shmat(shmid,NULL,0);
        if(shared_data==(int *)-1)
        {
                perror("shmat");
                return 1;
        }
        for(int i=0;i<4;i++)
        {
                shared_data[i]=0;
        }
        printf("Parent:shared memory initialized.\n");
        for(int i=0;i<3;i++)
        {
                pid_t pid=fork();
                if(pid==0)
                {
                        shared_data[i]=(i+1)*10;
                        printf("child %d wrote %d into shared memory.\n",i+1,shared_data[i]);
                        shmdt(shared_data);
                        exit(0);
                }
        }
        for(int i=0;i<3;i++)
        {
                wait(NULL);
        }
        sleep(1);
        printf("\nparent reading shared memory:\n");
        for(int i=0;i<3;i++)
        {
                printf("Value:%d=%d\n",i+1,shared_data[i]);
        }
        shmdt(shared_data);
        shmctl(shmid,IPC_RMID,NULL);
        return 0;
}
```
## 13.Write a program that dynamically creates shared memory segments based on user input.
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/shm.h>

int main() {
    key_t key;
    int shmid, size;
    void *shared_memory;

    printf("Enter a key value (integer): ");
    scanf("%d", &key);

    printf("Enter shared memory size in bytes: ");
    scanf("%d", &size);

    shmid = shmget(key, size, 0666 | IPC_CREAT);
    if (shmid == -1) {
        perror("shmget");
        return 1;
    }

    shared_memory = shmat(shmid, NULL, 0);
    if (shared_memory == (void *)-1) {
        perror("shmat");
        return 1;
    }

    printf("Shared memory created successfully!\n");
    printf("Shared memory ID: %d\n", shmid);
    printf("Attached address: %p\n", shared_memory);

    printf("\nEnter a message to store in shared memory: ");
    getchar();
    fgets((char *)shared_memory, size, stdin);

    printf("\nData stored successfully!\n");

    if (shmdt(shared_memory) == -1) {
        perror("shmdt");
        return 1;
    }

    printf("Shared memory detached successfully.\n");

    return 0;
}
```
## 14.Create a program that monitors and displays the usage statistics of shared memorysegments, such as the amount of memory used and the number of attached processes.
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <time.h>

int main() {
    key_t key;
    int shmid;
    struct shmid_ds shm_info;

    printf("Enter the shared memory key (integer): ");
    scanf("%d", &key);


    shmid = shmget(key, 0, 0666);
    if (shmid == -1) {
        perror("shmget");
        return 1;
    }


    if (shmctl(shmid, IPC_STAT, &shm_info) == -1) {
        perror("shmctl");
        return 1;
    }

    printf("\nShared Memory Segment Statistics\n");
    printf("Shared Memory ID        : %d\n", shmid);
    printf("Key                     : %d\n", key);
    printf("Size (in bytes)         : %lu\n", shm_info.shm_segsz);
    printf("Number of attachments   : %lu\n", shm_info.shm_nattch);
    printf("Last attach time        : %s", ctime(&shm_info.shm_atime));
    printf("Last detach time        : %s", ctime(&shm_info.shm_dtime));
    printf("Last change time        : %s", ctime(&shm_info.shm_ctime));
    printf("Owner UID               : %u\n", shm_info.shm_perm.uid);
    printf("Owner GID               : %u\n", shm_info.shm_perm.gid);
    printf("Creator PID             : %d\n", shm_info.shm_cpid);
    printf("Last operation PID      : %d\n", shm_info.shm_lpid);
    printf("\n");

    return 0;
}
```
## 15.Design a program that dynamically resizes a shared memory segment based on changing requirements.
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<unistd.h>
int main()
{
        key_t key=ftok(".",'R');
        int size;

        printf("Enter initial size of shared memory(in bytes):");
        scanf("%d",&size);

        int shmid=shmget(key,size,0666|IPC_CREAT);
        if(shmid==-1)
        {
                perror("shmget");
                exit(1);
        }
        printf("Shared memory of size %d bytes created.\n",size);
        while(1)
        {
                printf("\nDo you want to resize shared memory?(y/)n:\n");
                char ch;
                scanf(" %c",&ch);
                if(ch=='n')
                        break;
                printf("Enter new size(in bytes):");
                scanf("%d",&size);

                shmctl(shmid,IPC_RMID,NULL);
                shmid=shmget(key,size,0666|IPC_CREAT);
                if(shmid==-1)
                {
                        perror("shmget");
                        exit(1);
                }
                printf("Reszied shared memory to %d bytes.\n",size);
        }
        shmctl(shmid,IPC_RMID,NULL);
        printf("shared memory removed.\n");
        return  0;
}
```
## 16.Write a C program that uses semget to create a new semaphore set
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/sem.h>
int main()
{
        key_t key=ftok(".",'S');
        int semid=semget(key,1,0666|IPC_CREAT);
        if(semid==-1)
        {
                perror("semget");
                return 1;
        }
        printf("Semaphore set created with ID:%d\n",semid);
        return 0;
}

```
## 17.Enhance the program to initialize the values of the semaphores(binary semaphore) in the set using semctl.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/sem.h>
int main()
{
        key_t key=ftok(",",'B');
        int semid=semget(key,1,0666|IPC_CREAT);
        if(semid==-1)
        {
                perror("semget");
                return 1;
        }
        if(semctl(semid,0,SETVAL,1)==-1)
        {
                perror("semctl");
                return 1;
        }
        printf("Binary semaphore initialized with value1.\n");
        return 0;
}
```
## 18.Design a program where multiple processes are forked and synchronization is achieved using semaphores.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/sem.h>
#include<sys/wait.h>
#include<unistd.h>

void wait_semaphore(int semid)
{
        struct sembuf sb={0,-1,0};
        semop(semid,&sb,1);
}
void signal_semaphore(int semid)
{
        struct sembuf sb={0,1,0};
        semop(semid,&sb,1);
}
int main()
{
        key_t key=ftok(".",'S');
        int semid=semget(key,1,0666|IPC_CREAT);
        semctl(semid,0,SETVAL,1);
        for(int i=0;i<3;i++)
        {
                if(fork()==0)
                {
                        wait_semaphore(semid);
                        printf("Child %d in critical section\n",i+1);
                        sleep(1);
                        printf("child %d leaving critical section\n",i+1);
                        signal_semaphore(semid);
                        return 0;
                }
        }
        for(int i=0;i<3;i++)
        {
                wait(NULL);
        }
        semctl(semid,0,IPC_RMID);
        return 0;
}
```
## 19.Write a program that combines semaphores and shared memory for synchronization between processes.
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    key_t key1 = ftok(".", 'M');
    key_t key2 = ftok(".", 'S');

    int shmid = shmget(key1, sizeof(int), 0666 | IPC_CREAT);
    int *shared_data = shmat(shmid, NULL, 0);
    *shared_data = 0;

    int semid = semget(key2, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, 1);

    struct sembuf wait_op = {0, -1, 0};
    struct sembuf signal_op = {0, 1, 0};

    for (int i = 0; i < 3; i++) {
        if (fork() == 0) {
            semop(semid, &wait_op, 1);
            (*shared_data)++;
            printf("Child %d incremented value: %d\n", i + 1, *shared_data);
            semop(semid, &signal_op, 1);
            exit(0);
        }
    }

    for (int i = 0; i < 3; i++)
        wait(NULL);

    printf("Final value in shared memory: %d\n", *shared_data);

    shmdt(shared_data);
    shmctl(shmid, IPC_RMID, NULL);
    semctl(semid, 0, IPC_RMID);
    return 0;
}
```
## 20.Create a multithreaded program where threads synchronize using semaphore sets.
```c
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>

sem_t sem;

void* task(void* arg) {
    sem_wait(&sem);
    printf("Thread %ld in critical section\n", (long)arg);
    sleep(1);
    sem_post(&sem);
    return NULL;
}

int main() {
    pthread_t t[3];
    sem_init(&sem, 0, 1);

    for (long i = 0; i < 3; i++)
        pthread_create(&t[i], NULL, task, (void*)i);

    for (int i = 0; i < 3; i++)
        pthread_join(t[i], NULL);

    sem_destroy(&sem);
    return 0;
}
```
## 21.Create a program with two processes that perform semaphore operations (wait and signal) on a semaphore set using semop.
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    key_t key = ftok(".", 'X');
    int semid = semget(key, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, 1);

    struct sembuf wait_op = {0, -1, 0};
    struct sembuf signal_op = {0, 1, 0};

    if (fork() == 0) {
        semop(semid, &wait_op, 1);
        printf("Child in critical section\n");
        sleep(2);
        semop(semid, &signal_op, 1);
        return 0;
    } else {
        sleep(1);
        semop(semid, &wait_op, 1);
        printf("Parent in critical section\n");
        semop(semid, &signal_op, 1);
        wait(NULL);
        semctl(semid, 0, IPC_RMID);
    }
}
```
## 22.Write a program that performs control operations on the semaphore set using semctl.
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>

int main() {
    key_t key = ftok(".", 'C');
    int semid = semget(key, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, 3);

    int val = semctl(semid, 0, GETVAL);
    printf("Semaphore current value: %d\n", val);

    semctl(semid, 0, SETVAL, 1);
    val = semctl(semid, 0, GETVAL);
    printf("Semaphore value changed to: %d\n", val);

    semctl(semid, 0, IPC_RMID);
    return 0;
}
```
## 23.. Develop a C program that implements a simple dining philosophers problem using semaphores for synchronization.
```c
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

#define N 5

sem_t chopstick[N];

void* philosopher(void* num) {
    int i = *(int*)num;

    sem_wait(&chopstick[i]);
    sem_wait(&chopstick[(i + 1) % N]);

    printf("Philosopher %d is eating\n", i);
    sleep(1);

    sem_post(&chopstick[i]);
    sem_post(&chopstick[(i + 1) % N]);

    printf("Philosopher %d finished eating\n", i);
    return NULL;
}

int main() {
    pthread_t tid[N];
    int phil[N];

    for (int i = 0; i < N; i++) {
        sem_init(&chopstick[i], 0, 1);
        phil[i] = i;
    }

    for (int i = 0; i < N; i++)
        pthread_create(&tid[i], NULL, philosopher, &phil[i]);

    for (int i = 0; i < N; i++)
        pthread_join(tid[i], NULL);

    for (int i = 0; i < N; i++)
        sem_destroy(&chopstick[i]);

    return 0;
}
```
## 24.Write a program where multiple processes compete for access to a critical section using semaphores to ensure mutual exclusion.
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    key_t key = ftok(".", 'M');
    int semid = semget(key, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, 1);

    struct sembuf wait_op = {0, -1, 0};
    struct sembuf signal_op = {0, 1, 0};

    for (int i = 0; i < 3; i++) {
        if (fork() == 0) {
            semop(semid, &wait_op, 1);
            printf("Process %d entered critical section\n", i + 1);
            sleep(1);
            printf("Process %d leaving critical section\n", i + 1);
            semop(semid, &signal_op, 1);
            return 0;
        }
    }

    for (int i = 0; i < 3; i++)
        wait(NULL);

    semctl(semid, 0, IPC_RMID);
}
```
## 25.Write a C program to create a pipe and pass an array of integers from the parent process to the child process through the pipe.
```c
#include<stdio.h>
#include<unistd.h>
#include<sys/wait.h>

int main()
{
        int fd[2];
        pipe(fd);

        if(fork()==0)
        {
                close(fd[1]);
                int arr[5];
                read(fd[0],arr,sizeof(arr));
                printf("Child received array:");
                for(int i=0;i<5;i++)
                {
                        printf("%d",arr[i]);
                }
                printf("\n");
                close(fd[0]);
        }
        else
        {
                close(fd[0]);
                int arr[5]={1,2,3,4,5};
                write(fd[1],arr,sizeof(arr));
                close(fd[1]);
                wait(NULL);
        }
        return 0;
}
```
## 26.Implement a program where multiple child processes are created, and each child process communicates with the parent process using pipes.
```c
#include<stdio.h>
#include<unistd.h>
#include<sys/wait.h>
#include<string.h>

int main()
{
        int n=3;
        int fd[2];
        pipe(fd);
        for(int i=0;i<n;i++)
        {
                if(fork()==0)
                {
                        close(fd[0]);
                        char msg[100];
                        sprintf(msg,"Child %d says hi\n",i+1);
                        write(fd[1],msg,strlen(msg));
                        close(fd[1]);
                        _exit(0);
                }
        }
        close(fd[1]);
        char buf[256];
        int r;
        printf("Parent reading messages:\n");
        while((r=read(fd[0],buf,sizeof(buf)-1))>0)
        {
                buf[r]=0;
                printf("%s",buf);
        }
        close(fd[0]);
        for(int i=0;i<n;i++)
                wait(NULL);
        return 0;
}
```
## 27.Develop a program that uses pipes for bidirectional communication between two processes, where each process can send and receive messages.
```c
#include<stdio.h>
#include<unistd.h>
#include<sys/wait.h>
#include<string.h>

int main()
{
        int p2c[2],c2p[2];
        pipe(p2c);
        pipe(c2p);
        if(fork()==0)
        {
                close(p2c[1]);
                close(c2p[0]);
                char msg[100];
                read(p2c[0],msg,sizeof(msg));
                printf("child received:%s\n",msg);
                sprintf(msg,"Hello parent");
                write(c2p[1],msg,strlen(msg)+1);
                close(p2c[0]);
                close(c2p[1]);
                _exit(0);
        }
        else
        {
                close(p2c[0]);
                close(c2p[1]);
                char msg[]="Hello child";
                write(p2c[1],msg,strlen(msg)+1);
                char buf[100];
                read(c2p[0],buf,sizeof(buf));
                printf("parent got:%s\n",buf);
                close(p2c[1]);
                close(c2p[0]);
                wait(NULL);
        }
        return 0;
}
```
## 28.Create a C program where multiple processes write data to a named pipe, and another process reads from the named pipe and displays the received data.
### writer
```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

#define FIFO "myfifo"

int main() {
    int fd = open(FIFO, O_WRONLY);
    char msg[50];
    sprintf(msg, "Message from PID %d\n", getpid());
    write(fd, msg, strlen(msg) + 1);
    close(fd);
    return 0;
}
```
### reader
```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>

#define FIFO "myfifo"

int main() {
    mkfifo(FIFO, 0666);
    int fd = open(FIFO, O_RDONLY);
    char buf[100];
    printf("Reader started:\n");
    while (read(fd, buf, sizeof(buf)) > 0)
        printf("%s", buf);
    close(fd);
    unlink(FIFO);
    return 0;
}
```
## 29.Implement a program where two processes exchange messages through a named pipe until a termination signal is received.
```c
