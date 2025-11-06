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
/*
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<errno.h>

void main()
{
        struct stat buf;
        char* path="/home/sreevas2003/lsp/file_mgmt/test";
        if(stat(path,&buf)==-1)
        {
                perror("");
                exit(EXIT_FAILURE);
        }
        if(S_ISDIR(buf.st_mode))
                printf("directory is present\n");
        else
                printf("directory is not present\n");
}
*/
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
## 13. Write a C program to recursively list all files and directories in a given directory?
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<fcntl.h>
#include<dirent.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<errno.h>
void path_indentation(int depth)
{
        for(int i=0;i<depth;i++)
                printf("|   ");
}
int list(char *path,int depth)
{
        DIR* dir;
        struct stat buf;
        struct dirent *entry;
        if(!(dir=opendir(path)))
        {
                perror("Details:");
                return EXIT_FAILURE;
        }
        while((entry=readdir(dir))!=0)
        {
                char full_path[4096];
                if(snprintf(full_path,4096,"%s/%s",path,entry->d_name)>=4096)
                {
                        perror("Details:");
                        continue;
                }
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                if(stat(full_path,&buf)==-1)
                {
                        perror("Details:");
                        continue;
                }
                path_indentation(depth);
                if(S_ISDIR(buf.st_mode))
                {
                        printf("|__ [DIR] %s\n",entry->d_name);
                        list(full_path,depth+1);
                }
                else if(S_ISREG(buf.st_mode))
                {
                        printf("|-- [FILE] %s\n",entry->d_name);
                }
                else
                {
                        printf("|-- [OTHER] %s\n",entry->d_name);
                }
        }
        closedir(dir);
        return EXIT_SUCCESS;
}
void main(int argc,char *argv[])
{
        if(argc!=2)
        {
                perror("Details:");
                exit(EXIT_FAILURE);
        }
        printf("%s directory of list of files and sub directories\n",argv[1]);
        if(list(argv[1],0)==0)
        {
                printf("\nsuccessfully completed\n");
        }
        else
        {
                perror("Details:");
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
```c
#include<stdio.h>
#include<stdlib.h>
#include<dirent.h>
#include<sys/stat.h>
#include<errno.h>
#include<string.h>
#include<unistd.h>
off_t get_dir_size(char* path)
{
        DIR* dir;
        struct dirent *entry;
        struct stat buf;
        off_t to=0;
        dir = opendir(path);
        if(dir==0)
        {
                perror("Details:");
                return -1;
        }
        while((entry=readdir(dir))!=NULL)
        {
                char full[1024];
                if((strcmp(entry->d_name,".")==0) || (strcmp(entry->d_name,"..")==0))
                        continue;
                snprintf(full,sizeof(full),"%s/%s",path,entry->d_name);
                if(stat(full,&buf)==-1)
                {
                        perror("");
                        continue;
                }
                if(S_ISDIR(buf.st_mode))
                {
                        off_t sub=get_dir_size(full);
                        if(sub!=-1)
                                to+=sub;
                }
                else if(S_ISREG(buf.st_mode))
                        to+=buf.st_size;
        }
        closedir(dir);
        return to;
}
```
## 26. Implement a C program to recursively copy all files and directories from one directory to another?
```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>
#include <fcntl.h> 
#include <unistd.h> 
#include <errno.h>  
#define PATH_MAX 4096
#define BUFFER_SIZE 4096
int copy_file(const char *src_path, const char *dest_path, mode_t mode) {
    int src_fd, dest_fd;
    ssize_t bytes_read, bytes_written;
    char buffer[BUFFER_SIZE];
    src_fd = open(src_path, O_RDONLY);
    if (src_fd < 0) {
        perror("Error opening source file for reading");
        return -1;
    }
    dest_fd = open(dest_path, O_WRONLY | O_CREAT | O_TRUNC, 0666);
    if (dest_fd < 0) {
        perror("Error opening destination file for writing");
        close(src_fd);
        return -1;
    }
    while ((bytes_read = read(src_fd, buffer, BUFFER_SIZE)) > 0) {
        bytes_written = write(dest_fd, buffer, bytes_read);
        if (bytes_written != bytes_read) {
            perror("Error writing to destination file (incomplete write)");
            close(src_fd);
            close(dest_fd);
            return -1;
        }
    }
    if (bytes_read == -1) {
        perror("Error reading from source file");
        close(src_fd);
        close(dest_fd);
        return -1;
    }
    if (fchmod(dest_fd, mode) == -1) {
        perror("Warning: Error setting file permissions on destination");
    }
    close(src_fd);
    close(dest_fd);
    printf("[FILE] Copied: %s\n", dest_path);
    return 0;
}
int copy_recursively(char *src_path,char *dest_path) {
    DIR *dir;
    struct dirent *entry;
    struct stat buf;
    char next_src_path[PATH_MAX];
    char next_dest_path[PATH_MAX];
    if (stat(src_path, &buf) == -1) {
        perror("Error getting status of source path");
        return -1;
    }
    if (!S_ISDIR(buf.st_mode)) {
        fprintf(stderr, "Error: Internal call made with non-directory source: %s\n", src_path);
        return -1;
    }
    if (mkdir(dest_path, buf.st_mode) == -1) {
        if (errno != EEXIST) {
            perror("Error creating destination directory");
            return -1;
        }
    } else {
         printf("[DIR] Created: %s\n", dest_path);
    }
    if (!(dir = opendir(src_path))) {
        perror("Error opening source directory for reading");
        return -1;
    }
    while ((entry = readdir(dir)) != NULL) {
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0) {
            continue;
        }
        if (snprintf(next_src_path, PATH_MAX, "%s/%s", src_path, entry->d_name) >= PATH_MAX ||
            snprintf(next_dest_path, PATH_MAX, "%s/%s", dest_path, entry->d_name) >= PATH_MAX) {
            fprintf(stderr, "Warning: Path buffer overflow for entry %s. Skipping.\n", entry->d_name);
            continue;
        }
        if (stat(next_src_path, &buf) == -1) {
            perror("Error getting status of entry");
            continue;
        }
        if (S_ISDIR(buf.st_mode)) {
            if (copy_recursively(next_src_path, next_dest_path) == -1) {
                closedir(dir);
                return -1;
            }
        } else if (S_ISREG(buf.st_mode)) {
            copy_file(next_src_path, next_dest_path, buf.st_mode);
        }
    }
    closedir(dir);
    return 0;
}
int main(int argc, char *argv[]) {
    if (argc != 3) {
        fprintf(stderr, "Usage: %s <source_directory> <destination_directory>\n", argv[0]);
        return 1;
    }
    char *source_dir = argv[1];
    char *destination_dir = argv[2];
    printf("Starting recursive copy process...\n");
    printf("Source: %s\n", source_dir);
    printf("Destination: %s\n", destination_dir);
    if (strncmp(source_dir, destination_dir, strlen(source_dir)) == 0 && (destination_dir[strlen(source_dir)] == '/' || destination_dir[strlen(source_dir)] == '\0')) {
        fprintf(stderr, "Error: Destination directory cannot be inside the source directory.\n");
        return 1;
    }
    if (copy_recursively(source_dir, destination_dir) == 0) {
        printf("\nRecursive copy completed successfully.\n");
        return 0;
    } else {
        fprintf(stderr, "\nRecursive copy failed due to an error.\n");
        return 1;
    }
}
```
## 27. Write a C program to get the number of files in a directory named "Images"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<dirent.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<errno.h>
int count(char* path)
{
        int c=0;
        DIR* dir;
        struct stat buf;
        struct dirent *entry;
        if((dir=opendir(path))==0)
        {
                perror("Details:");
                return -1;
        }
        while((entry=readdir(dir))!=NULL)
        {
                char full_path[4096];
                if(snprintf(full_path,4096,"%s/%s",path,entry->d_name)>=4096)
                {
                        perror("Details:");
                        continue;
                }
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                if(stat(full_path,&buf)==-1)
                {
                        perror("Dteails:");
                        continue;
                }
                if(S_ISREG(buf.st_mode))
                        c++;
        }
        closedir(dir);
        return c;
}
void main()
{
        char *path="/home/sreevas2003/lsp";
        int c=count(path);
        if(c==-1)
                perror("Details:");
        else
                printf("Number of files in LSP directory is %d\n",c);
}
```
## 28. Develop a C program to create a FIFO (named pipe) named "myfifo" in the current directory? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<errno.h>
#include<sys/types.h>
#include<sys/stat.h>
void main()
{
        mode_t per=0666;
        umask(0);
        if(mkfifo("myfifo",per)==-1)
        {
                if(errno==EEXIST)
                {
                        printf("Already existn\n");
                        exit(EXIT_FAILURE);
                }
                else
                {
                        perror("Details:");
                        exit(EXIT_FAILURE);
                }
        }
        printf("FIFO created successfully\n");
        exit(EXIT_SUCCESS);
}
```
## 29. Implement a C program to read data from a FIFO named "myfifo"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<errno.h>
#include<sys/types.h>
#include<fcntl.h>
void main()
{
        int fd;
        char buf[200];
        if((fd=open("myfifo",O_RDONLY))<0)
        {
                perror("Details:");
                exit(1);
        }
        int n;
        if((n= read(fd,buf,sizeof(buf)))<0)
        {
                perror("Details:");
                exit(1);
        }
        buf[n]='\0';
        printf("Contents of FIFO is :\n%s\n",buf);
        close(fd);
        exit(0);
}
/*
 echo "Hello FIFO!" > myfifo
 */
```
## 30. Write a C program to truncate a file named "file.txt" to a specified length? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<errno.h>
void main()
{
        int len;
        printf("Enter length: ");
        scanf("%d",&len);
        if(truncate("file.txt",len)==-1)
        {
                perror("Details:");
                exit(1);
        }
        printf("File '%s' is turncated in '%d' length\n","file.txt",len);
        exit(0);
}
```
## 31. Develop a C program to search for a specific string in a file named "data.txt" and display the line number(s) where it occurs? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<fcntl.h>
#include<sys/types.h>
void main()
{
        int fd;
        if((fd=open("file.txt",O_RDONLY))<0)
        {
                perror("Details:");
                exit(1);
        }
        FILE *fp = fdopen(fd, "r");
        char str[30];
        printf("Enter a string: ");
        scanf("%s",str);
        char line[4096];
        int line_no=0;
        int flag=0;
        while(fgets(line,sizeof(line),fp))
        {
                line_no++;
                if(strstr(line,str)!=0)
                {
                        printf("%s string found at line number is %d\n",line,line_no);
                        flag=1;
                }
        }
        if(!flag)
        {
                perror("Details:");
                exit(1);
        }
        close(fd);
        exit(0);
}
```
## 32. Implement a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path?
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
#include<errno.h>
int main(void)
{
    const char *path = "file.txt";
    struct stat buf;

    if (lstat(path, &buf) == -1) {
        perror("lstat");
        return 1;
    }

    if (S_ISLNK(buf.st_mode))
        printf("It is a symbolic link\n");
    else if (S_ISREG(buf.st_mode))
        printf("It is a regular file\n");
    else if (S_ISDIR(buf.st_mode))
        printf("It is a directory\n");
    else if (S_ISCHR(buf.st_mode))
        printf("It is a character device\n");
    else if (S_ISBLK(buf.st_mode))
        printf("It is a block device\n");
    else if (S_ISFIFO(buf.st_mode))
        printf("It is a FIFO/pipe\n");
    else if (S_ISSOCK(buf.st_mode))
        printf("It is a socket\n");
    else
        printf("Unknown type\n");

    return 0;
}
```
## 33. Write a C program to create a new empty file named "empty.txt"? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/types.h>
void main()
{
        int fd;
        if((fd=open("empty.txt",O_CREAT,0640))<0)
        {
                perror("Details:");
                exit(1);
        }
        printf("Empty file Created successfully\n");
}
```
## 34. Develop a C program to get the permissions (mode) of a file named "file.txt"? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<errno.h>
void main()
{
        const char* path="file.txt";
        struct stat buf;
        if(stat(path,&buf)==1)
        {
                perror("Details:");
                exit(1);
        }
        printf("Permissions of file %s : ",path);
        printf((S_ISDIR(buf.st_mode))?"d":"-");
        printf((buf.st_mode & S_IRUSR)?"r":"-");
        printf((buf.st_mode & S_IWUSR)?"w":"-");
        printf((buf.st_mode & S_IXUSR)?"x":"-");
        printf((buf.st_mode & S_IRGRP)?"r":"-");
        printf((buf.st_mode & S_IWGRP)?"w":"-");
        printf((buf.st_mode & S_IXGRP)?"x":"-");
        printf((buf.st_mode & S_IROTH)?"r":"-");
        printf((buf.st_mode & S_IWOTH)?"w":"-");
        printf((buf.st_mode & S_IXOTH)?"x":"-");
        printf("\n");
}
```
## 35. Implement a C program to recursively delete a directory named "Temp" and all its contents? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>
void deleteDirectory(const char *path) {
    struct dirent *entry;
    DIR *dp = opendir(path);
    if (dp == NULL) {
        perror("Unable to open directory");
        return;
    }
    char fullPath[1024];
    struct stat statbuf;
    while ((entry = readdir(dp)) != NULL) {
        if (strcmp(entry->d_name, ".") == 0 ||strcmp(entry->d_name, "..") == 0)
            continue;
        sprintf(fullPath, "%s/%s", path, entry->d_name);
        if (stat(fullPath, &statbuf) == 0) {
            if (S_ISDIR(statbuf.st_mode)) {
                deleteDirectory(fullPath);
            }
            if (remove(fullPath) != 0) {
                perror("Error deleting");
            }
        }
    }
    closedir(dp);
    if (remove(path) != 0) {
        perror("Error removing directory");
    }
}
int main() {
    const char *dirName = "dsp";
    printf("Deleting directory: %s\n", dirName);
    deleteDirectory(dirName);
    printf("Deletion completed.\n");
    return 0;
}
```
## 36. Write a C program to read and display the first 10 lines of a file named "log.txt"? 
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<errno.h>
#include<sys/types.h>
#include<fcntl.h>
void main()
{
        int fd;
        fd=open("file.txt",O_RDONLY);
        FILE* fp= fdopen(fd,"r");
        char buf[1024];
        int c=0;
        while(fgets(buf,sizeof(buf),fp)!=NULL && c<10)
        {
                printf("%s",buf);
                c++;
        }
        close(fd);
}
```
## 37. Develop a C program to read data from a text file named "input.txt" and write it to another file named "output.txt" in reverse order? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
int main(){
        int fdin,fdout;
        fdin=open("input.txt",O_RDONLY);
        if(fdin<0){
                printf("Error");
                exit(1);
        }
        char str[100];
        int bytes;
        bytes=read(fdin,str,sizeof(str)-1);
        if(bytes<0){
                printf("Error");
                close(fdin);
                exit(1);
        }
        str[bytes]='\0';
        close(fdin);
        for(int i=0,j=strlen(str)-1;i<j;i++,j--){
                int temp=str[i];
                str[i]=str[j];
                str[j]=temp;
        }
        fdout=open("output.txt",O_WRONLY|O_CREAT,0666);
        if(fdout<0){
                printf("Error");
                exit(1);
        }
        if(write(fdout,str,strlen(str))<0){
                printf("error");
                exit(1);
        }
        close(fdout);
        printf("File reversed successfully\n");

}
```
## 38. Implement a C program to create a new directory named with the current date in the format "YYYY-MM-DD"? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <sys/stat.h>
#include <sys/types.h>
int main() {
    time_t now = time(NULL);
    struct tm *t = localtime(&now);
    char dirname[20];
    strftime(dirname, sizeof(dirname), "%Y-%m-%d", t);
    if (mkdir(dirname, 0755) == -1) {
        perror("mkdir error");
        return 1;
    }
    printf("Directory '%s' created successfully.\n", dirname);
    return 0;
}
```
## 39. Write a C program to read and display the contents of a binary file named "binary.bin"? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd;
    unsigned char buffer[1];
    ssize_t bytes;
    fd = open("binary.bin", O_RDONLY);
    if (fd < 0) {
        perror("Error opening binary.bin");
        exit(1);
    }
    printf("Contents of binary.bin (in hex):\n\n");
    while ((bytes = read(fd, buffer, 1)) > 0) {
        printf("%02X ", buffer[0]);
    }
    if (bytes < 0) {
        perror("Read error");
        close(fd);
        exit(1);
    }
    printf("\n");
    close(fd);
    return 0;
}
```
## 40. Develop a C program to get the size of the largest file in a directory? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<dirent.h>
#include<sys/stat.h>
#include<errno.h>
void main(int argc,char* argv[])
{
        if(argc!=2)
        {
                perror("error");
                exit(1);
        }
        DIR* dir;
        struct stat buf;
        struct dirent *entry;
        int large=-1;
        if((dir=opendir(argv[1]))==NULL)
        {
                perror("error");
                exit(1);
        }
        char full_path[1024];
        while((entry=readdir(dir))!=NULL)
        {
                if(strcmp(entry->d_name,".")==0||strcmp(entry->d_name,"..")==0)
                        continue;
                snprintf(full_path,sizeof(full_path),"%s/%s",argv[1],entry->d_name);
                if(stat(full_path,&buf)==-1)
                {
                        perror("error");
                        exit(1);
                }
                if(S_ISREG(buf.st_mode))
                {
                        if(buf.st_size>large)
                                large=buf.st_size;
                }
        }
        closedir(dir);
        printf("largest file size is %d\n",large);
}
```
## 41. Implement a C program to check if a file named "data.txt" is readable? 
```c
#include <stdio.h>
#include <unistd.h>
int main() {
    if (access("file.txt", R_OK) == 0) {
        printf("File '%s' is readable.\n", "file.txt");
    } else {
        perror("File is not readable");
    }
    return 0;
}
```
## 42. Write a C program to create a new directory named "Logs" and move all files with the ".log" extension into it?
```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <string.h>
#include <sys/stat.h>
#include <unistd.h>
int main() {
    DIR *dir;
    struct dirent *entry;
    if (mkdir("Logs", 0755) == -1) {
        perror("mkdir");
    }
    if ((dir=opendir(".")) == NULL) {
        perror("opendir");
        return 1;
    }
    while ((entry = readdir(dir)) != NULL) {
        if (entry->d_type == DT_DIR)
            continue;
        char *name = entry->d_name;
        int len = strlen(name);
        if (len > 4 && strcmp(name + (len - 4), ".log") == 0) {
            char src[256], dest[256];
            snprintf(src, sizeof(src), "%s", name);
            snprintf(dest, sizeof(dest), "Logs/%s", name);
            if (rename(src, dest) == -1)
                perror("rename");
            else
                printf("Moved: %s -> %s\n", src, dest);
        }
    }
    closedir(dir);
    return 0;
}
```
## 43. Develop a C program to check if a file named "config.ini" is writable? 
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
void main()
{
        if(access("file.txt",W_OK)==0)
                printf("file %s is Writable\n","file.txt");
        else
                perror("Details:");
}
```
## 44. Implement a C program to read the contents of a text file named "instructions.txt" and execute the instructions as shell commands? 
```c
#include <fcntl.h>
#include <unistd.h>
#include <string.h>
#include <stdlib.h>
#include <stdio.h>

int main() {
    int fd = open("instructions.txt", O_RDONLY);
    if (fd < 0) {
        perror("open");
        return 1;
    }

    char buf[1024];
    ssize_t n = read(fd, buf, sizeof(buf) - 1);
    close(fd);
    if (n <= 0) return 0;

    buf[n] = '\0';
    char *cmd = strtok(buf, "\n");
    while (cmd) {
        system(cmd); // executes shell command
        cmd = strtok(NULL, "\n");
    }

    return 0;
}
```
## 45.Write a C program to get the number of hard links to a file named "file.txt"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
int main(){
        struct stat buf;
        if(stat("file.txt",&buf)==-1){
                printf("Error");
                exit(1);
        }
        printf("Number of hard links to 'file.txt' :%lu\n",buf.st_nlink);
}
```
## 46. Develop a C program to copy the contents of all text files in a directory into a single file named "combined.txt"? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <string.h>
#include <sys/stat.h>
int ends_with_txt(const char *name) {
    int len = strlen(name);
    return (len > 4 && strcmp(name + len - 4, ".txt") == 0);
}
int main() {
    DIR *dir;
    struct dirent *entry;
    FILE *out, *in;
    char buffer[1024];
    out = fopen("combined.txt", "w");
    if (!out) {
        perror("fopen combined.txt");
        return 1;
    }
    dir = opendir(".");
    if (!dir) {
        perror("opendir");
        fclose(out);
        return 1;
    }
    while ((entry = readdir(dir)) != NULL) {
        if (entry->d_type == DT_DIR)
            continue;
        const char *name = entry->d_name;
        if (ends_with_txt(name) && strcmp(name, "combined.txt") != 0) {
            in = fopen(name, "r");
            if (!in) {
                perror("fopen input file");
                continue;
            }
            size_t n;
            while ((n = fread(buffer, 1, sizeof(buffer), in)) > 0) {
                fwrite(buffer, 1, n, out);
            }
            fclose(in);
            fprintf(out, "\n");
            printf("Copied %s\n", name);
        }
    }
    closedir(dir);
    fclose(out);
    printf("All text files combined into combined.txt\n");
    return 0;
}
```
## 47. Implement a C program to recursively calculate the total size of all files in a directory and its subdirectories? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>
long long get_size(const char *path) {
    DIR *dir;
    struct dirent *entry;
    struct stat st;
    char fullpath[1024];
    long long total = 0;
    dir = opendir(path);
    if (!dir) {
        perror("opendir");
        return 0;
    }
    while ((entry = readdir(dir)) != NULL) {
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;
        snprintf(fullpath, sizeof(fullpath), "%s/%s", path, entry->d_name);
        if (lstat(fullpath, &st) == -1) {
            perror("lstat");
            continue;
        }
        if (S_ISDIR(st.st_mode)) {
            total += get_size(fullpath);
        }
        else if (S_ISREG(st.st_mode)) {
            total += st.st_size;
        }
    }
    closedir(dir);
    return total;
}
int main(int argc, char *argv[]) {
    if (argc != 2) {
        printf("Usage: %s <directory>\n", argv[0]);
        return 1;
    }
    long long total = get_size(argv[1]);
    printf("Total size: %lld bytes\n", total);
    return 0;
}
```
## 48. Write a C program to get the number of bytes in a file named "data.bin"? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
int main(){
        int fd;
        fd=open("data.bin",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        int size=lseek(fd,0,SEEK_END);
        if(size==-1){
                perror("Error in getting file size");
                close(fd);
                exit(1);
        }
        printf("Number of bytes in 'data.bin':%d\n",size);
        close(fd);
}
```
## 49. Develop a C program to create a new directory named with the current timestamp in the format "YYYY-MM-DD-HH-MM-SS"? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <sys/stat.h>
#include <sys/types.h>
int main() {
    time_t now = time(NULL);
    struct tm *t = localtime(&now);
    char dirname[32];
    strftime(dirname, sizeof(dirname), "%Y-%m-%d-%H-%M-%S", t);
    if (mkdir(dirname, 0755) == -1) {
        perror("mkdir");
        return 1;
    }
    printf("Directory '%s' created successfully.\n", dirname);
    return 0;
}
```
## 50. Write a C program to create a symbolic link named "link.txt" to a file named "target.txt"? 
```c
#include <stdio.h>
#include <unistd.h>
int main() {
    const char *target = "target.txt";
    const char *linkname = "link.txt";
    if (symlink(target, linkname) == -1) {
        perror("symlink");
        return 1;
    }
    printf("Symbolic link '%s' → '%s' created successfully.\n",
           linkname, target);
    return 0;
}
```
## 51. Develop a C program to count the number of words in a file named "essay.txt"? 
```c
#include <stdio.h>
#include <ctype.h>
int main() {
    FILE *fp = fopen("essay.txt", "r");
    if (!fp) {
        perror("fopen");
        return 1;
    }
    int c, prev = ' ';
    int words = 0;
    while ((c = fgetc(fp)) != EOF) {
        if (isspace(prev) && !isspace(c)) {
            words++;
        }
        prev = c;
    }
    fclose(fp);
    printf("Total words: %d\n", words);
    return 0;
}
```
## 52. Write a C program to get the last access time of a file named "data.txt"? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<time.h>
int main(){
        const char *filename="data.txt";
        struct stat buf;
        if(stat(filename,&buf)>0){
                printf("Error");
                exit(1);
        }
        printf("Last access time:%s\n",ctime(&buf.st_atime));
}
```
