## 1.Write a C program to create a thread that prints "Hello, World!"? 
```c
#include<stdio.h>
#include<string.h>
#include<pthread.h>
#include<stdlib.h>
void* fun(void *arg);
void main()
{
        pthread_t ti;
        int res;
        pthread_create(&ti,NULL,fun,"Hello, World!");
        pthread_join(ti,&res);
        printf("%d\n",res);
}
void* fun(void *arg)
{
        char *sptr;
        sptr=(char*)arg;
        printf("%s\n",sptr);
        int re=strlen(sptr);
        return re;
}
```
## 2.Modify the above program to create multiple threads, each printing its own message? 
```c
