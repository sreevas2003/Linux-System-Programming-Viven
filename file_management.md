## 1. Write a C program to create a new text file and write "Hello, World!" to it? 
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/types.h>
void main()
{
        int fd;
        char buf[50];
        printf("Enter a string");
        gets(buf);
        fd=open("file.txt",O_WRONLY|O_CREAT,0640);
        write(fd,buf,strlen(buf));

}
```
## 2. Develop a C program to open an existing text file and display its contents? 
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<fcntl.h>
#include<string.h>
void main()
{
        int fd,res;
        char buf[50];
        fd=open("file.txt",O_RDONLY,0);
        if(fd<0)
        {
                printf("file is not open\n");
                exit(1);
        }
        read(fd,buf,50);
        printf("%s\n",buf);
}
```
## 3 Implement a C program to create a new directory named "Test" in the current directory?
```c
//cmd:- $ mkdir test
#include<stdio.h>
#include<sys/stat.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/types.h>
void main()
{
        int res=mkdir("test",0777);
        if(res==0)
                puts("sub directory created in main directory\n");
        else
                puts("An error occured\n");
}
```
## 4. Write a C program to check if a file named "sample.txt" exists in the current directory?
```c
#include<stdio.h>
#include<fcntl.h>
#include<sys/types.h>
void main()
{
        int fd;
        fd=open("sample.txt",O_RDONLY,0);
        if(fd<0)
                printf("file doesn't exist\n");
        else
                printf("file is found\n");
}
```
## 5. Develop a C program to rename a file from "oldname.txt" to "newname.txt"?
```c
//cmd:- mv newname.txt oldname.txt
#include<stdio.h>
void main()
{
        if(rename("destfile.txt","destnationfile.txt")==0)
                printf("file is renamed\n");
        else
                printf("oldfile not exist\n");
}
```
## 6. Implement a C program to delete a file named "delete_me.txt"? 
```c
#include<stdio.h>
void main()
{
        if(remove("destfile.txt")==0)
                printf("File is delete successfully\n");
        else
                printf("File is not found or deleted\n");
}
```
## 7. Write a C program to copy the contents of one file to another?
```c
//cmd:- $cp destfile.txt file.txt
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
void main()
{
        int sfd,dfd,num;
        char buf[100];
        sfd=open("file.txt",O_RDONLY);
        dfd=open("destfile.txt",O_WRONLY|O_CREAT,0640);
        while((num=read(sfd,buf,100))>0)
                write(dfd,buf,num);
        close(sfd);
        close(dfd);
}
```
## 8. Develop a C program to move a file from one directory to another?
```c
```
## 9. Implement a C program to list all files in the current directory? 
```c
//cmd:- $ls -l
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
void main()
{
        //execl("/bin/ls","ls","-l",0);
        execlp("ls","ls","-l",0);
/*
        char* argv[5];
        argv[0]="ls";
        argv[1]="-l";
        argv[2]='\0';
//      execv("/bin/ls",argv);
        execvp("ls",argv);
*/
}
```
## 10. Write a C program to get the size of a file named "file.txt"? 
```c
