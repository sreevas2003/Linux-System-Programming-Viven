## 1. Write a C program to create a new text file and write "Hello, World!" to it? 
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
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
#include<stdio.h>
#include<errno.h>
void main()
{
        char *old="/home/sreevas2003/lsp/file_mgmt/dump.txt";
        char *new="/home/sreevas2003/lsp/process_mgmt/renew.txt";
        if(rename(old,new)==0)
                printf("Successfully changed\n");
        else
        {
                printf("not changed\n");
                perror("details:");
        }
}
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
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<sys/stat.h>
#include<sys/types.h>
void main()
{
        struct stat buf;
        int fd=open("file.txt",O_RDONLY);
        if(fd<0)
        {
                printf("file doesnot exist\n");
                exit(EXIT_FAILURE);
        }
        fstat(fd,&buf);
        printf("file size is : %d\n",buf.st_size);
        close(fd);
}
```
## 11. Develop a C program to check if a directory named "Test" exists in the current directory? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<sys/types.h>
void main()
{
        int fd;
        fd=open("test",O_RDONLY);
        if(fd<0)
                printf("directory is not present\n");
        else
                printf("directory is present\n");
}
```
## 12. Implement a C program to create a new directory named "Backup" in the parent directory? 
```c
#include<stdio.h>
#include<errno.h>
#include<sys/stat.h>
void main()
{
        if(mkdir("/home/sreevas2003/lsp/backup",0777)==0)
                printf("Successfully backup parent directory is created\n");
        else
        {
                printf("Not created\n");
                perror("details:");
        }
}
```
## 14. Develop a C program to delete all files in a directory named "Temp"?
```c
#include<stdio.h>
#include<errno.h>
void main()
{
        if(remove("temp")==0)
                printf("Succefully deleted\n");
        else
        {
                printf("not found\n");
                perror("details:");
        }
}
```
## 15. Implement a C program to count the number of lines in a file named "data.txt"? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<sys/types.h>
#include<unistd.h>
void main()
{
        int fd,c=0;
        char buf[500];
        ssize_t ch;
        fd=open("file.txt",O_RDONLY);
        if(fd<0)
        {
                printf("Error occured\n");
                exit(EXIT_FAILURE);
        }
        while((ch=read(fd,buf,500))>0)
        {
                for(int i=0;i<ch;i++)
                {
                        if(buf[i]=='\n')
                                c++;
                }
        }
        printf("total lines in this file is %d\n",c);
        close(fd);
}
```
## 16. Write a C program to append "Goodbye!" to the end of an existing file named "message.txt"?
```c
#include<stdio.h>
#include<fcntl.h>
#include<sys/types.h>
#include<unistd.h>
#include<string.h>
void main()
{
        int fd;
        char buf[50];
        printf("Enter appended string");
        gets(buf);
        int s=strlen(buf);
        fd=open("sample.txt",O_WRONLY|O_APPEND,0);
        write(fd,buf,s);
        close(fd);
}
```
## 17. Implement a C program to change the permissions of a file named "file.txt" to read only? 
```c
#include<stdio.h>
#include<fcntl.h>
#include<stdlib.h>
#include<sys/types.h>
#include<unistd.h>
void main()
{
        printf("before changing permisssion\n");// $ ls -l
        sleep(5);
        if(chmod("file.txt",0444)==0)
        {
                printf("permissions changed successfull\n");
                exit(EXIT_SUCCESS);
        }
        else
        {
                printf("not changed\n");
                exit(EXIT_FAILURE);
        }
}
```
## 18. Write a C program to change the ownership of a file named "file.txt" to the user "user1"? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <pwd.h>      
#include <errno.h>    
int main() {
    const char *filename = "file.txt";
    const char *target_username = "sreevas2003";
    struct passwd *pwd_info;
    uid_t target_uid;
    gid_t target_gid;
    pwd_info = getpwnam(target_username);
    if (pwd_info == NULL)
    {
        fprintf(stderr, "Error: User '%s' not found on this system (getpwnam failed).\n", target_username);
        exit(EXIT_FAILURE);
    }
    target_uid = pwd_info->pw_uid;
    target_gid = pwd_info->pw_gid; 
    printf("Attempting to change ownership of '%s' to user '%s'...\n",filename, target_username);
    printf("Target UID: %d, Target GID: %d\n", (int)target_uid, (int)target_gid);
    if (chown(filename, target_uid, target_gid) == 0) {
        printf("Success: Ownership of '%s' changed to user '%s'.\n", filename, target_username);
    } else {
        perror("Error changing ownership (chown)");
        fprintf(stderr, "Failed to change ownership. Ensure you run this with superuser privileges (e.g., 'sudo').\n");
        exit(EXIT_FAILURE);
    }
    exit(EXIT_SUCCESS);
}
```
## 19. Develop a C program to get the last modified timestamp of a file named "file.txt"? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <time.h>
#include <unistd.h>
int main() {
    const char *filename = "file.txt";
    struct stat buf;
    if (stat(filename, &buf) == -1) {
        perror("Error getting file status");
        return EXIT_FAILURE;
    }
    time_t time = buf.st_mtime;
    char *time_str = ctime(&time);
    printf("File: %s\n", filename);
    printf("Last modified timestamp (time_t): %ld\n", (long)time);
    printf("Last modified time (formatted): %s", time_str);
    return EXIT_SUCCESS;
}
```
## 20. Implement a C program to create a temporary file and write some data to it? 
```c
#include<stdio.h>
#include<fcntl.h>
#include<stdlib.h>
#include<sys/types.h>
void main()
{
        int fd;
        fd=open("tempfile.txt",O_RDWR|O_CREAT,0640);
        char buf[]="this is a temperary file";
        int n=sizeof(buf);
        write(fd,buf,n);
}
```
## 21. Write a C program to check if a given path refers to a file or a directory? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<errno.h>
#include<unistd.h>
void main()
{
        char *path="/home/sreevas2003/lsp/file_mgmt/15.c";
        struct stat buf;
        if(stat(path,&buf)==-1)
        {
                perror("details:");
                exit(EXIT_FAILURE);
        }
        if(S_ISREG(buf.st_mode))
                printf("It is a Regular file\n");
        else if(S_ISDIR(buf.st_mode))
                printf("It is a directory\n");
        else
                printf("It is another ipc object\n");
}
```
## 22. Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt"? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<errno.h>
void main()
{
        if(link("file.txt","link.txt")==0)
        {
                printf("Successfully '%s' linked to '%s' file\n","file.txt","link.txt");
                exit(EXIT_SUCCESS);
        }
        else
        {
                perror("details :");
                exit(EXIT_FAILURE);
        }
}
```
## 23. Implement a C program to read and display the contents of a CSV file named "data.csv"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<sys/types.h>
#include<unistd.h>
#include<errno.h>
void main()
{
        int fd;
        fd=open("file.csv",O_RDONLY,0640);
        if(fd<0)
        {
                perror("Details:");
                exit(EXIT_FAILURE);
        }
        char buf[200];
        size_t len=sizeof(buf);
        read(fd,buf,len);
        buf[len]='\0';
        printf("contents of csv file: \n%s",buf);
}
```
## 24. Write a C program to get the absolute path of the current working directory?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<errno.h>
void main()
{
        char cwd[4096];
        if(getcwd(cwd,sizeof(cwd))!=0)
        {
                printf("we get path Successfully\n");
                printf("%s\n",cwd);
                exit(EXIT_SUCCESS);
        }
        else
        {
                perror("error Details:\n");
                exit(EXIT_FAILURE);
        }
}
```
## 25. Develop a C program to get the size of a directory named "Documents"? 
