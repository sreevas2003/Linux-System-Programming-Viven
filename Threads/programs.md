## 1.Write a C program to create a thread that prints Hello world.
```c
#include <stdio.h>
#include <pthread.h>
void *helloworld(void *args);
int main(void)
{
    pthread_t t1;
    pthread_create(&t1, NULL, helloworld, NULL);
    pthread_join(t1, NULL);
    return 0;
}
void *helloworld(void *args)
{
    printf("Hello world\n");
    return NULL;
}
```
## 2.Modify the above program to create multiple threads, each printing its own message.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
void *thread1(void *args)
{
        printf("Hello\n");
        sleep(1);
        return NULL;
}
void *thread2(void *args)
{
        printf("I am learning linux\n");
        sleep(1);
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        //to create threads
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## Program to create two threads that prints numbers from 1 to 10 concurrently.
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<pthread.h>
void *thread1(void *args)
{
        int i=1;
        for(i=1;i<=10;i++)
        {
                printf("Thread1:%d\n",i);
                sleep(1);
        }
        return NULL;
}
void *thread2(void *args)
{
        int i=1;
        for(i=1;i<=10;i++)
        {
                printf("Thread2:%d\n",i);
                sleep(1);
        }
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## 4.Implement a thread that calculates the factorial of a number
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
void *factorial(void *args)
{
        int n= *(int *)args;
        long long int fact=1;
        for(int i=1;i<=n;i++)
        {
                fact=fact*i;
        }
        long long *result=(long long *)malloc(sizeof(long long));
        *result=fact;
        return result;
}
int main()
{
        pthread_t ti;
        int num;
        printf("enter a number to find factorial:\n");
        scanf("%d",&num);
        void *res;

        pthread_create(&ti,NULL,factorial,&num);
        pthread_join(ti,&res);

        long long *factresult=(long long*)res;

        printf("factorial of %d is %lld:\n",num,*factresult);
        free(factresult);
        return 0;
}
```
## 5.Write a program to create two threads that print their thread ids.
```c
#include<stdio.h>
#include<unistd.h>
#include<pthread.h>
void *thread1(void *args)
{
        printf("hello\n");
        return NULL;
}
void *thread2(void *args)
{
        printf("world\n");
        return NULL;
}
int main()
{
        pthread_t t1,t2;

        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);

        printf("thread1 identifier:%lu\n",t1);
        printf("thread2 identifier:%lu\n",t2);

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## 6.Write a program to create a thread that prints sum of two numbers.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
#include<stdlib.h>
struct numbers
{
        int a;
        int b;
};
void *add(void *args)
{
        struct numbers *nums=(struct numbers *)args;
        int sum=nums->a+nums->b;
        printf("sum of %d and %d is: %d\n",nums->a,nums->b,sum);
        return NULL;
}
int main()
{
        pthread_t t1;
        struct numbers *n=malloc(sizeof(struct numbers));
        n->a=10;
        n->b=20;
        pthread_create(&t1,NULL,add,n);
        pthread_join(t1,NULL);
        free(n);
}
```
## 7.Implement a C programs that creates a thread that prints square of a number.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void *square(void *args) {
    int num = *(int *)args;
    int sq = num * num;

    int *result = (int *)malloc(sizeof(int));
    *result = sq;

    return result;
}

int main() {
    pthread_t t1;
    int n;

    printf("Enter a number:\n");
    scanf("%d", &n);

    void *res;
    pthread_create(&t1, NULL, square, &n);
    pthread_join(t1, &res);

    int *squareresult = (int *)res;
    printf("Square of %d is %d\n", n, *squareresult);

    free(squareresult);
    return 0;
}
```
## 8.Write a c program that creates a thread and  prints current date and time.
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<time.h>
void *printDateTime(void *args)
{
        time_t t;
        struct tm *current_time;
        time(&t);
        current_time=localtime(&t);
        printf("Current date and time:%s",asctime(current_time));
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,printDateTime,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 9.Write a c program that creates a thread that calculates given number is prime or not.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
void *prime(void *args)
{
        int num=*(int *)args;
        int isprime=1;
        for(int i=2;i<num;i++)
        {
                if(num%i==0)
                {
                        isprime=0;
                        break;
                }
        }
        if(isprime)
        {
                printf("The given number is a prime number\n");
        }
        else
        {
                printf("The given number is not a prime number\n");
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        int n;
        scanf("%d",&n);
        pthread_create(&t1,NULL,prime,&n);
        pthread_join(t1,NULL);
        return 0;
}
```
## 10.Implement a C program that checks whether a string is a palindrome or not using threads.
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>

void *checkPalindrome(void *args) {
    char *str = (char *)args;
    int len = strlen(str);
    int *result = (int *)malloc(sizeof(int));

    *result = 1;

    for (int i = 0; i < len / 2; i++) {
        if (str[i] != str[len - i - 1]) {
            *result = 0;
            break;
        }
    }

    return result;
}

int main() {
    pthread_t t1;
    char str[100];

    printf("Enter a string: ");
    scanf("%s", str);  // input string (no spaces)

    void *res;
    pthread_create(&t1, NULL, checkPalindrome, str);
    pthread_join(t1, &res);

    int *isPalindrome = (int *)res;
    if (*isPalindrome)
        printf("The string '%s' is a palindrome.\n", str);
    else
        printf("The string '%s' is not a palindrome.\n", str);

    free(isPalindrome);
    return 0;
}
```
## 11.Write a C program to create a thread that prints Hello world with thread synchronisation.
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t mtx;
void *thread(void *arg)
{
        pthread_mutex_lock(&mtx);
        printf("Hello world\n");
        pthread_mutex_unlock(&mtx);
        return NULL;
}
void main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread,NULL);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
}
```
## 12.Develop a C program that creates two threads that prints their IDs and synchronize their outputs.
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t mtx;
void *thread1(void *args)
{
        pthread_mutex_lock (&mtx);
        printf("Hello\n");
        pthread_mutex_unlock(&mtx);
        return NULL;
}
void *thread2(void *args)
{
        pthread_mutex_lock(&mtx);
        printf("World\n");
        pthread_mutex_unlock(&mtx);
        return NULL;
}
void main()
{
        pthread_t t1,t2;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);
        printf("First thread identifier: %lu\n",t1);
        printf("Second thread identifier: %lu\n",t2);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        pthread_mutex_destroy(&mtx);
}
```
## 13.Implement a C program to create a thread that generates random numbers and synchronizes access to a shared buffer.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
#include<stdlib.h>
#include<time.h>
#define SIZE 10
int buff[SIZE];
int k=0;
pthread_mutex_t mtx;
void *thread(void *arg)
{
        srand(time(NULL));
        for(int i=0;i<SIZE;i++)
        {
                pthread_mutex_lock(&mtx);
                int num=rand()%100;
                buff[k++]=num;
                printf("%d\n",num);
                pthread_mutex_unlock(&mtx);
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread,NULL);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 14.Write a C program to create a thread that performs addition of two numbers with mutex locks?
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
#include<stdlib.h>
struct numbers
{
        int a;
        int b;
};
pthread_mutex_t mtx;
void *add(void *args)
{
        struct numbers *nums=(struct numbers *)args;
        pthread_mutex_lock(&mtx);
        int sum=nums->a+nums->b;
        printf("sum of %d and %d is: %d\n",nums->a,nums->b,sum);
        pthread_mutex_unlock(&mtx);
        return NULL;
}
int main()
{
        pthread_t t1;
        struct numbers *n=malloc(sizeof(struct numbers));
        n->a=10;
        n->b=20;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,add,n);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
        free(n);
}
```
## 15.Implement a C program to create two threads that increment and decrement a shared variable,respectively,using mutex locks?
```c
#include<stdio.h>
#include<pthread.h>
int sh_var=0;
pthread_mutex_t mtx;
void *thread1(void *args)
{
        int i=0;
        for(i=0;i<5;i++)
        {
                pthread_mutex_lock(&mtx);
                sh_var++;
                printf("the shared variable in incrementing thread:%d\n",sh_var);
                pthread_mutex_unlock(&mtx);
        }
}
void *thread2(void *args)
{
        int i=0;
        for(i=0;i<5;i++)
        {
                pthread_mutex_lock(&mtx);
                sh_var--;
                printf("The shared variable in decrementing thread:%d\n",sh_var);
                pthread_mutex_unlock(&mtx);
        }
}
int main()
{
        pthread_t t1,t2;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 16.Develop a C program to create two threads that reads input from the user and synchronizes access to shared resources?
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t mtx;
char str[100];
void *userinput(void *args)
{
        pthread_mutex_lock(&mtx);
        printf("Enter input:\n");
        fgets(str,100,stdin);
        printf("user input:%s",str);
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,userinput,NULL);
        pthread_join(t1,NULL);
        printf("User given input in mainthread:%s",str);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 17.Implement a C program to create a thread that prints prime numbers up to a given limit with mutex locks
```c
#include<stdio.h>
#include<pthread.h>
#include<math.h>
pthread_mutex_t mtx;
void *prime(void *args)
{
        printf("The prime numbers in the range are:\n");
        for(int n=2;n<10;n++)
        {
                int count=0;
                for(int i=2;i<=n/2;i++)
                {
                        if(n%i==0)
                        {
                                count++;
                                break;
                        }
                }
                if(count==0)
                {
                        pthread_mutex_lock(&mtx);
                        printf("%d\n",n);
                        pthread_mutex_unlock(&mtx);
                }
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,prime,NULL);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 18.Implement a C program to create a thread that calculates the sum of Fibonacci series up to a given limit using dynamic programming with mutex locks.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

pthread_mutex_t mtx;
int *fibs;
size_t count = 0;
size_t capacity = 10;
int sum = 0;
int limit = 10;

void *range_fib(void *args)
{
    int a = 0, b = 1, c;
    fibs = malloc(capacity * sizeof(int));
    if (!fibs) {
        printf("Malloc error\n");
        return NULL;
    }

    while (a <= limit) {
        if (count == capacity) {
            capacity *= 2;
            fibs = realloc(fibs, capacity * sizeof(int));
            if (!fibs) {
                printf("Realloc error\n");
                return NULL;
            }
        }
        pthread_mutex_lock(&mtx);
        fibs[count++] = a;
        sum += a;
        pthread_mutex_unlock(&mtx);

        c = a + b;
        a = b;
        b = c;
    }
    return NULL;
}

int main()
{
    pthread_t t1;
    pthread_mutex_init(&mtx, NULL);

    pthread_create(&t1, NULL, range_fib, NULL);
    pthread_join(t1, NULL);

    printf("Fibonacci numbers up to %d: ", limit);
    for (size_t i = 0; i < count; i++) {
        printf("%d ", fibs[i]);
    }
    printf("\nFinal sum = %d\n", sum);

    pthread_mutex_destroy(&mtx);
    free(fibs);
    return 0;
}
```
## 19.Write a C program to create a thread that checks if a given year is a leap year using dynamic programming with mutex locks.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
pthread_mutex_t mtx;
void *leap_year(void *args)
{
    int *year = malloc(sizeof(int));
    if (!year) {
        printf("Malloc error\n");
        return NULL;
    }
    *year = *(int *)args;
    int is_leap = (*year % 400 == 0) || ((*year % 100 != 0) && (*year % 4 == 0));
    pthread_mutex_lock(&mtx);
    printf("%d is %s leap year.\n", *year, is_leap ? "a" : "not a");
    pthread_mutex_unlock(&mtx);
    free(year);
    return NULL;
}

int main()
{
    pthread_t t1;
    int in_year;
    printf("Enter a year: ");
    scanf("%d", &in_year);
    pthread_mutex_init(&mtx, NULL);
    pthread_create(&t1, NULL, leap_year, &in_year);
    pthread_join(t1, NULL);

    pthread_mutex_destroy(&mtx);
    return 0;
}
```
## 20.Write a C program to create a thread that checks if a string is palindrome using dynamic programming with mutex locks?
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>
pthread_mutex_t mtx;
void *check_palindrome(void *args) {
    char *str = malloc(100 * sizeof(char));
    if (!str) {
        printf("Malloc error\n");
        return NULL;
    }
    printf("Enter a string: ");
    scanf("%s", str);

    int n = strlen(str);
    int is_palindrome = 1;

    for (int i = 0; i < n / 2; i++) {
        if (str[i] != str[n - i - 1]) {
            is_palindrome = 0;
            break;
        }
    }
    pthread_mutex_lock(&mtx);
    if (is_palindrome)
        printf("\"%s\" is a palindrome.\n", str);
    else
        printf("\"%s\" is not a palindrome.\n", str);
    pthread_mutex_unlock(&mtx);

    free(str);
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_mutex_init(&mtx, NULL);
    pthread_create(&t1, NULL, check_palindrome, NULL);
    pthread_join(t1, NULL);

    pthread_mutex_destroy(&mtx);
    return 0;
}
```
## 21.Implement a C program to create a thread that performs selection sort on an array of integers.
```c
#include<stdio.h>
#include<pthread.h>
void *sort(void *args)
{
        printf("enter number of elements:\n");
        int size;
        int i,j;
        scanf("%d",&size);
        int arr[size];
        printf("Enter elements of an array:\n");
        for(i=0;i<size;i++)
        {
                scanf("%d",&arr[i]);
        }
        for(i=0;i<size;i++)
        {
                int min=i;
                for(j=i+1;j<size;j++)
                {
                        if(arr[j]<arr[min])
                        {
                                min=j;
                        }
                }
                int temp=arr[i];
                arr[i]=arr[min];
                arr[min]=temp;
        }
        printf("Sorted array:");
        for(int i=0;i<size;i++)
        {
                printf("%d ",arr[i]);
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,sort,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 22.Develop a C to create a thread that calculates area of a triangle.
```c
#include<stdio.h>
#include<pthread.h>
void *area_of_triangle(void *args)
{
        double height,breadth;
        printf("Enter height and breadth of a triangle:\n");
        scanf("%lf %lf",&height,&breadth);
        double area=0.5*height*breadth;
        printf("The area of the triangle is:%.2lf\n",area);
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,area_of_triangle,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 23.Write a C program to create a thread that calculates the sum of squares of numbers from 1 to 100.
```c
#include <stdio.h>
#include <pthread.h>
void *sum_of_squares(void *args) {
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
        sum += i * i;
    }
    printf("Sum of squares from 1 to 100 is: %d\n", sum);
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, sum_of_squares, NULL);
    pthread_join(t1, NULL);
    return 0;
}
```
## 24.Write a C program to create a thread that generated a random array of numbers.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>

void *generate_random_array(void *args) {
    int n;
    printf("Enter the size of the array: ");
    scanf("%d", &n);
    int arr[n];
    srand(time(NULL));

    printf("Random array: ");
    for (int i = 0; i < n; i++) {
        arr[i] = rand() % 100; // random number between 0-99
        printf("%d ", arr[i]);
    }
    printf("\n");
    return NULL;
}
int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, generate_random_array, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 25.Implement a C program to create a thread that performs bubble sortt on an array of integers.
```c
oid *bubble_sort(void *args) {
    int n;
    printf("Enter the size of the array: ");
    scanf("%d", &n);

    int arr[n];
    printf("Enter %d elements: ", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");

    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, bubble_sort, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 26.Develop a C program to create a thread that calculates greatest common divisor(GCD) of two numbers?
```c
#include <stdio.h>
#include <pthread.h>
void *calculate_gcd(void *args) {
    int a, b;
    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    int x = a, y = b;
    while (y != 0) {
        int temp = y;
        y = x % y;
        x = temp;
    }

    printf("GCD of %d and %d is: %d\n", a, b, x);
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, calculate_gcd, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 27.Implement a C program that calculates sum of even numbers from 1 to 100.
```c
#include <stdio.h>
#include <pthread.h>

void *sum_of_even(void *args) {
    int sum = 0;
    for (int i = 2; i <= 100; i += 2) {
        sum += i;
    }

    printf("Sum of even numbers from 1 to 100 is: %d\n", sum);
    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, sum_of_even, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 28.Implement a C program that performs multiplication of two matrices.
```c
#include <stdio.h>
#include <pthread.h>

void *matrix_multiply(void *args) {
    int r1, c1, r2, c2;

    printf("Enter rows and columns of first matrix: ");
    scanf("%d %d", &r1, &c1);
    printf("Enter rows and columns of second matrix: ");
    scanf("%d %d", &r2, &c2);

    if (c1 != r2) {
        printf("Matrix multiplication not possible! Columns of first must equal rows of second.\n");
        return NULL;
    }

    int mat1[r1][c1], mat2[r2][c2], result[r1][c2];

    printf("Enter elements of first matrix:\n");
    for (int i = 0; i < r1; i++)
        for (int j = 0; j < c1; j++)
            scanf("%d", &mat1[i][j]);

    printf("Enter elements of second matrix:\n");
    for (int i = 0; i < r2; i++)
        for (int j = 0; j < c2; j++)
            scanf("%d", &mat2[i][j]);

    // Initialize result matrix to 0
    for (int i = 0; i < r1; i++)
        for (int j = 0; j < c2; j++)
            result[i][j] = 0;

    // Matrix multiplication
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++) {
            for (int k = 0; k < c1; k++) {
                result[i][j] += mat1[i][k] * mat2[k][j];
            }
        }
    }

    // Print result
    printf("Resultant matrix:\n");
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++)
            printf("%d ", result[i][j]);
        printf("\n");
    }

    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, matrix_multiply, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 29.Develop a C program that calculates the average of numbers from 1 to 100.
```c
#include <stdio.h>
#include <pthread.h>

void *calculate_average(void *args) {
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
        sum += i;
    }

    double average = sum / 100.0;  // divide by total count
    printf("Average of numbers from 1 to 100 is: %.2lf\n", average);

    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, calculate_average, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 30.Write a C program to create a thread that generates a random string.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>
#include <string.h>

void *generate_random_string(void *args) {
    int length;
    printf("Enter the length of the random string: ");
    scanf("%d", &length);

    char str[length + 1];
    const char charset[] = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

    srand(time(NULL));

    for (int i = 0; i < length; i++) {
        int key = rand() % (int)(sizeof(charset) - 1);
        str[i] = charset[key];
    }
    str[length] = '\0';

    printf("Random string: %s\n", str);

    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, generate_random_string, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 31.Write a C program to create a thread that checks if a given number is a perfect square.
```c
#include<stdio.h>
#include<pthread.h>
void *perfect_square(void *args)
{
        int n;
        scanf("%d",&n);
        int is_perfect=0;
        for(int i=1;i*i<=n;i++)
        {
                if(i*i==n)
                {
                        is_perfect=1;
                        break;
                }
        }
        if(is_perfect)
        {
                printf("The given number is a perfect square\n");
        }
        else
        {
                printf("The given number is not a perfect square\n");
        }
        return 0;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,perfect_square,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 32.Write a C program to calculate the sum of digits of a given number.
```c
#include<stdio.h>
#include<pthread.h>
void *sum_of_digits(void *args)
{
        int num=*(int *)args;
        int sum=0;
        while(num)
        {
                int rem=num%10;
                sum=sum+rem;
                num=num/10;
        }
        printf("The sum of digits are:%d\n",sum);
        return 0;
}
int main()
{
        pthread_t t1;
        int n;
        printf("Enter a number:\n");
        scanf("%d",&n);
        pthread_create(&t1,NULL,sum_of_digits,&n);
        pthread_join(t1,NULL);
        return 0;
}
```
## 33.Implement a C program to create a thread that calculates the factorial of a given number using recursion
```c
#include <stdio.h>
#include <pthread.h>

int max_value;
int n;
void *find_max(void *args)
{
    int *arr = (int *)args;
    max_value = arr[0];

    for (int i = 1; i < n; i++)
    {
        if (arr[i] > max_value)
            max_value = arr[i];
    }
    return NULL;
}
int main()
{
    pthread_t t1;

    printf("Enter size of array: ");
    scanf("%d", &n);

    int arr[n];
    printf("Enter %d elements:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    pthread_create(&t1, NULL, find_max, arr);
    pthread_join(t1, NULL);

    printf("Maximum element = %d\n", max_value);
    return 0;
}
```
## 34.Develop a C program to create a thread that finds the maximum element in an array.
```c
#include <stdio.h>
#include <pthread.h>

int max_value;
int n;
void *find_max(void *args)
{
    int *arr = (int *)args;
    max_value = arr[0];

    for (int i = 1; i < n; i++)
    {
        if (arr[i] > max_value)
            max_value = arr[i];
    }
    return NULL;
}
int main()
{
    pthread_t t1;

    printf("Enter size of array: ");
    scanf("%d", &n);

    int arr[n];
    printf("Enter %d elements:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    pthread_create(&t1, NULL, find_max, arr);
    pthread_join(t1, NULL);

    printf("Maximum element = %d\n", max_value);
    return 0;
}
```
## 35.Write a C program That sorts an array of strings.
```c
#include<stdio.h>
#include<pthread.h>
#include <stdio.h>
#include <string.h>

int main()
{
    int n;
    printf("Enter number of strings: ");
    scanf("%d", &n);
    char arr[n][100];
    printf("Enter %d strings:\n", n);
    for (int i = 0; i < n; i++)
    {
        scanf("%s", arr[i]);
    }
    for (int i = 0; i < n - 1; i++)
    {
        for (int j = i + 1; j < n; j++)
        {
            if (strcmp(arr[i], arr[j]) > 0)
            {
                char temp[100];
                strcpy(temp, arr[i]);
                strcpy(arr[i], arr[j]);
                strcpy(arr[j], temp);
            }
        }
    }

    printf("\nSorted strings:\n");
    for (int i = 0; i < n; i++)
    {
        printf("%s\n", arr[i]);
    }

    return 0;
}
```
## 36.Write a C program to create a thread that checks if a number is even or odd.
```c
#include<stdio.h>
#include<pthread.h>
void *even_odd(void *args)
{
        int num=*(int *)args;
        if(num%2==0)
        {
                printf("%d is an even number\n",num);
        }
        else
        {
                printf("%d is an odd number\n",num);
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        printf("Enter a number:\n");
        int n;
        scanf("%d",&n);
        pthread_create(&t1,NULL,even_odd,&n);
        pthread_join(t1,NULL);
        return 0;
}
```
## 37.Implement a C program to create a thread that calculates the average of elements in an array.
```c
#include <stdio.h>
#include <pthread.h>

double sum = 0;
int n;

void *calculate_sum(void *args)
{
    double *arr = (double *)args;
    sum = 0;
    for (int i = 0; i < n; i++)
        sum += arr[i];
    double average=sum/n;
    printf("Average=%.2lf\n",average);
    return NULL;
}

int main()
{
    printf("Enter the number of elements: ");
    scanf("%d", &n);
    double arr[n];
    printf("Enter %d numbers:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%lf", &arr[i]);
    pthread_t t1;
    pthread_create(&t1, NULL, calculate_sum, arr);
    pthread_join(t1, NULL);
    return 0;
}
```
## 38.Write a C program to create a thread that generates a random password.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>
#include <string.h>

#define PASSWORD_LENGTH 12  // length of password

char password[PASSWORD_LENGTH + 1];  // global variable to store password

void *generate_password(void *args)
{
    const char charset[] = "abcdefghijklmnopqrstuvwxyz"
                           "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
                           "0123456789"
                           "!@#$%^&*()_+-=<>?";
    int n = sizeof(charset) - 1;  // exclude null character

    for (int i = 0; i < PASSWORD_LENGTH; i++)
    {
        int index = rand() % n;   // pick random index
        password[i] = charset[index];
    }
    password[PASSWORD_LENGTH] = '\0';  // null terminate

    return NULL;
}

int main()
{
    srand(time(0));  // seed random number generator

    pthread_t t1;
    pthread_create(&t1, NULL, generate_password, NULL);
    pthread_join(t1, NULL);

    printf("Generated Password: %s\n", password);

    return 0;
}
```
## 39.Implement a C program to create a thread that calculates the area of a rectangle.
```c
include <stdio.h>
#include <pthread.h>

void *calculate_area(void *args)
{
    double *arr = (double *)args;
    double area = arr[0] * arr[1];
    printf("Area of rectangle = %.2lf\n", area);
    return NULL;
}

int main()
{
    pthread_t t1;
    double arr[2];

    printf("Enter length of rectangle: ");
    scanf("%lf", &arr[0]);

    printf("Enter width of rectangle: ");
    scanf("%lf", &arr[1]);

    pthread_create(&t1, NULL, calculate_area, arr);
    pthread_join(t1, NULL);

    return 0;
}
```
## 40.Develop a c program that creates a thread that calculates the average of numbers in an array.
```c
#include <stdio.h>
#include <pthread.h>

double sum = 0;
int n;

void *calculate_sum(void *args)
{
    double *arr = (double *)args;
    sum = 0;
    for (int i = 0; i < n; i++)
        sum += arr[i];
    double average=sum/n;
    printf("Average=%.2lf\n",average);
    return NULL;
}

int main()
{
    printf("Enter the number of elements: ");
    scanf("%d", &n);
    double arr[n];
    printf("Enter %d numbers:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%lf", &arr[i]);
    pthread_t t1;
    pthread_create(&t1, NULL, calculate_sum, arr);
    pthread_join(t1, NULL);
    return 0;
}
```
## 41.Develop a C program to create a thread that calculates the sum of elements in an array.
```c
#include<stdio.h>
#include<pthread.h>
int sum=0;
int n;
void *calculate_sum(void *args)
{
        int *arr=(int *)args;
        sum=0;
        for(int i=0;i<n;i++)
        {
                sum+=arr[i];
        }
        printf("The sum of elements of array is:%d\n",sum);
        return NULL;
}
int main()
{
        printf("Enter number of elements:\n");
        scanf("%d",&n);
        int arr[n];
        printf("Enter elements of the array:\n");
        for(int i=0;i<n;i++)
        {
                scanf("%d",&arr[i]);
        }
        pthread_t t1;
        pthread_create(&t1,NULL,calculate_sum,arr);
        pthread_join(t1,NULL);
        return 0;
}
```
## 42.Write a c program to create a thread that calculates the factorial of numbers from 1 to 10.
```c
#include<stdio.h>
#include<pthread.h>
void *fact(void *args)
{
        int num=*(int *)args;
        int fact;
        for(int n=1;n<=num;n++)
        {
                printf("The factorial of");
                fact=1;
                for(int i=1;i<=n;i++)
                {
                        fact=fact*i;
                }
                printf(" %d is: %d\n",n,fact);
        }
        return NULL;
}
int main()
{
        int num;
        printf("Range of number to calculate:\n");
        scanf("%d",&num);
        pthread_t t1;
        pthread_create(&t1,NULL,fact,&num);
        pthread_join(t1,NULL);
        return 0;
}
```
## 43.Write a C program to create a thread that searches for a given number in an array.
```c
#include<stdio.h>
#include<pthread.h>
int key;
int n;
void *search(void *args)
{
        int *arr=(int *)args;
        int is_found=0;
        printf("Enter element to search:\n");
        scanf("%d",&key);
        for(int i=0;i<n;i++)
        {
                if(arr[i]==key)
                {
                        is_found=1;
                }
        }
        if(is_found==1)
        {
                printf("The %d is present in the array.\n",key);
        }
        else
        {
                printf("The %d is not present in the array.\n",key);
        }
        return NULL;
}
int main()
{
        printf("Enter number of elements in an array:\n");
        scanf("%d",&n);
        printf("Enter elements of array:\n");
        int arr[n];
        for(int i=0;i<n;i++)
        {
                scanf("%d",&arr[i]);
        }
        pthread_t t1;
        pthread_create(&t1,NULL,search,&arr);
        pthread_join(t1,NULL);
        return 0;
}
```
## 44.Develop a C program that reverses a given string.
```c
#include<stdio.h>
#include<string.h>
#include<pthread.h>
void *revstr(void *args)
{
        char *str=(char *)args;
        int n=strlen(str);
        for(int i=n-1;i>=0;i--)
        {
                printf("%c",str[i]);
        }
        return NULL;
}
int main()
{
        char str[100];
        printf("Enter a string:\n");
        fgets(str,100,stdin);
        str[strcspn(str,"\n")]='\0';

        pthread_t t1;
        pthread_create(&t1,NULL,revstr,str);
        pthread_join(t1,NULL);
        return 0;
}
```
## 45.Develop a C program to create a thread that reads input from the user.
```c
#include <stdio.h>
#include <pthread.h>
void *read_input(void *args) {
    char str[100];
    printf("Thread: Enter a string:\n");
    fgets(str, sizeof(str), stdin);
    str[strcspn(str, "\n")] = '\0';

    printf("Thread: You entered -> %s\n", str);
    return NULL;
}
int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, read_input, NULL);
    pthread_join(t1, NULL);
    printf("Main: Thread finished execution.\n");
    return 0;
}
```
## 46.Write a C program to create a thread that performs addition of two matrices.
```c
#include <stdio.h>
#include <pthread.h>

#define MAX 10

int rows, cols;
int A[MAX][MAX], B[MAX][MAX], C[MAX][MAX];

void *add_matrices(void *args) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            C[i][j] = A[i][j] + B[i][j];
        }
    }
    printf("Resultant matrix after addition:\n");
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            printf("%d ", C[i][j]);
        }
        printf("\n");
    }
    return NULL;
}

int main() {
    printf("Enter number of rows and columns:\n");
    scanf("%d %d", &rows, &cols);

    printf("Enter elements of first matrix:\n");
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            scanf("%d", &A[i][j]);

    printf("Enter elements of second matrix:\n");
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            scanf("%d", &B[i][j]);

    pthread_t t1;
    pthread_create(&t1, NULL, add_matrices, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 47.Write a C program to create two threads using pthreads library.Each thread should print "Hello word" along with its thread ID.
```c
#include<stdio.h>
#include<pthread.h>
void *thread1(void *args)
{
        printf("Hello world\n");
        return NULL;
}
void *thread2(void *args)
{
        printf("HEllo world\n");
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        pthread_create(&t1,NULL,thread1,NULL);
        printf("thread1 identifier:%lu\n",(unsigned long)t1);
        pthread_create(&t2,NULL,thread2,NULL);
        printf("Thread2 identifier:%lu\n",(unsigned long)t2);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## 48.Develop a C program to create a thread that calculates the length of a given string.
```c
#include<stdio.h>
#include<string.h>
#include<pthread.h>
int len=0;
void *str_len(void *args)
{
        char str[100];
        fgets(str,100,stdin);
        str[strcspn(str,"\n")]='\0';
        for(int i=0;str[i]!='\0';i++)
        {
                len++;
        }
        printf("The length of the string is:%d\n",len);
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,str_len,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 49.Write a C program to create two threads using pthreads library.Each thread should print "Hello world" along with its thread ID.
```c
#include<stdio.h>
#include<pthread.h>
void *thread1(void *args)
{
        printf("Hello world\n");
        return NULL;
}
void *thread2(void *args)
{
        printf("HEllo world\n");
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        pthread_create(&t1,NULL,thread1,NULL);
        printf("thread1 identifier:%lu\n",(unsigned long)t1);
        pthread_create(&t2,NULL,thread2,NULL);
        printf("Thread2 identifier:%lu\n",(unsigned long)t2);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## 50. Modify the previous program to pass arguments to the threads and print those arguments along with the thread ID.
```c
#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <stdlib.h>

void *thread1(void *args)
{
    char *msg = (char *)args;
    printf("Thread1 says: %s\n", msg);
    printf("Thread1 ID: %lu\n", (unsigned long)pthread_self());
    return NULL;
}

void *thread2(void *args)
{
    char *msg = (char *)args;
    printf("Thread2 says: %s\n", msg);
    printf("Thread2 ID: %lu\n", (unsigned long)pthread_self());
    return NULL;
}

int main()
{
    pthread_t t1, t2;

    char *msg1 = "Hello from Thread 1";
    char *msg2 = "Hello from Thread 2";

    pthread_create(&t1, NULL, thread1, (void *)msg1);
    pthread_create(&t2, NULL, thread2, (void *)msg2);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}
```
## 50.Write a C program to demonstrate thread synchronization using mutex locks.Create two threads that increment a shared variable using mutex locks to ensure proper synchronization.
```c
#include<stdio.h>
#include<pthread.h>
int sh_var=0;
pthread_mutex_t mtx;
void *thread1(void *args)
{
        int i=0;
        for(i=0;i<5;i++)
        {
                pthread_mutex_lock(&mtx);
                sh_var++;
                printf("the shared variable in incrementing thread:%d\n",sh_var);
                pthread_mutex_unlock(&mtx);
        }
}
void *thread2(void *args)
{
        int i=0;
        for(i=0;i<5;i++)
        {
                pthread_mutex_lock(&mtx);
                sh_var--;
                printf("The shared variable in decrementing thread:%d\n",sh_var);
                pthread_mutex_unlock(&mtx);
        }
}
int main()
{
        pthread_t t1,t2;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 51.Extend the previous program to use semaphore using mutex locks.Create two threads that increment a shared variable using mutex locks to ensure proper synchronization.
```c
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>

int sh_var = 0;
sem_t sem;

void *thread1(void *args)
{
    for (int i = 0; i < 5; i++)
    {
        sem_wait(&sem);
        sh_var++;
        printf("The shared variable in incrementing thread: %d\n", sh_var);
        sem_post(&sem);
    }
    return NULL;
}

void *thread2(void *args)
{
    for (int i = 0; i < 5; i++)
    {
        sem_wait(&sem);
        sh_var--;
        printf("The shared variable in decrementing thread: %d\n", sh_var);
        sem_post(&sem);
    }
    return NULL;
}

int main()
{
    pthread_t t1, t2;
    sem_init(&sem, 0, 1);
    pthread_create(&t1, NULL, thread1, NULL);
    pthread_create(&t2, NULL, thread2, NULL);
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    sem_destroy(&sem);

    return 0;
}
```
## 52.Write a C program to implement the producer-consumer problem using pthreads.Create two threads-one for producing items and another for consuming items from a shared buffer?
```c
#include <stdio.h>
#include <pthread.h>

#define SIZE 5       // buffer size
#define N 10         // total items to produce/consume

int buf[SIZE];       // buffer
int count = 0;       // number of items in buffer
int in = 0, out = 0; // circular buffer indices

pthread_mutex_t lock;
pthread_cond_t notempty, notfull;

void *produce(void *arg)
{
    for (int item = 1; item <= N; item++)
    {
        pthread_mutex_lock(&lock);

        while (count == SIZE)
        {
            pthread_cond_wait(&notfull, &lock); // wait if buffer full
        }

        buf[in] = item;
        printf("Producer produced item: %d\n", item);
        in = (in + 1) % SIZE;
        count++;

        pthread_cond_signal(&notempty); // signal consumer
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

void *consume(void *arg)
{
    for (int i = 1; i <= N; i++)
    {
        pthread_mutex_lock(&lock);

        while (count == 0)
        {
            pthread_cond_wait(&notempty, &lock); // wait if buffer empty
        }

        int item = buf[out];
        printf("Consumer consumed item: %d\n", item);
        out = (out + 1) % SIZE;
        count--;

        pthread_cond_signal(&notfull); // signal producer
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

int main()
{
    pthread_t t1, t2;

    pthread_mutex_init(&lock, NULL);
    pthread_cond_init(&notempty, NULL);
    pthread_cond_init(&notfull, NULL);

    pthread_create(&t1, NULL, produce, NULL);
    pthread_create(&t2, NULL, consume, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&lock);
    pthread_cond_destroy(&notempty);
    pthread_cond_destroy(&notfull);

    printf("\nAll %d items produced and consumed successfully!\n", N);
    return 0;
}
```
## 53.Write a C program to demonstrate thread cancellation.Create a thread that runs infinite loop and cancels it after a certain condition is met from the main thread.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
#include<stdlib.h>
void *infi(void *arg)
{
        while(1)
        {
                printf("Thread running\n");
                sleep(1);
                pthread_testcancel();
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,infi,NULL);
        printf("cancel thread\n");
        pthread_cancel(t1);
        pthread_join(t1,NULL);
        return 0;
}
```
## 54.Write a C program to create a thread that prints the even numbers between 1 and 20.
```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>
#define LIMIT 20   
void *print_even(void *arg)
{
    for (int i = 1; i <= LIMIT; i++)
    {
        if(i%2==0)
                printf("Even: %d\n", i);
        sleep(1);
    }
    return NULL;
}
int main()
{
    pthread_t t1;
    pthread_create(&t1, NULL, print_even, NULL);
    pthread_join(t1, NULL);
    printf("Finished printing even numbers up to %d\n", LIMIT);
    return 0;
}
```
## 55.Develop a C program to create two threads that prints odd and even numbers alternatively.
```c
#include <pthread.h>
#define LIMIT 20   
pthread_mutex_t lock;
pthread_cond_t cond;
int counter = 1;
void *print_odd(void *arg) {
    while (counter <= LIMIT) {
        pthread_mutex_lock(&lock);
        while (counter % 2 == 0) {
            pthread_cond_wait(&cond, &lock);
        }
        if (counter <= LIMIT) {
            printf("Odd : %d\n", counter);
            counter++;
        }
        pthread_cond_signal(&cond);
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}
void *print_even(void *arg) {
    while (counter <= LIMIT) {
        pthread_mutex_lock(&lock);
        while (counter % 2 == 1) {
            pthread_cond_wait(&cond, &lock);
        }
        if (counter <= LIMIT) {
            printf("Even: %d\n", counter);
            counter++;
        }
        pthread_cond_signal(&cond);
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}
int main() {
    pthread_t t1, t2;
    pthread_mutex_init(&lock, NULL);
    pthread_cond_init(&cond, NULL);
    pthread_create(&t1, NULL, print_odd, NULL);
    pthread_create(&t2, NULL, print_even, NULL);
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    pthread_mutex_destroy(&lock);
    pthread_cond_destroy(&cond);
    printf("Finished printing numbers up to %d\n", LIMIT);
    return 0;
}
```
## 56.Implement a C program to create a thread that calculates the sum of squares of numbers from 1 to 10.
```c
#include<stdio.h>
#include<pthread.h>
int sum=0;
void *square(void *args)
{
        printf("The sum of square of numbers is:");
        int num=*((int *)args);
        for(int i=1;i<=num;i++)
        {
                int res=i*i;
                sum=sum+res;
                printf("After adding %d^2 = %d → sum = %d\n", i, res, sum);
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        int n=10;
        pthread_create(&t1,NULL,square,&n);
        pthread_join(t1,NULL);
        return 0;
}
```
## 57.Write a C program to create a thread that calculates the product of numbers from 1 to 10
```c
#include <stdio.h>
#include <pthread.h>
void *calculate_product(void *arg) {
    int product = 1;
    for (int i = 1; i <= 10; i++) {
        product *= i;
    }
    printf("The product of numbers from 1 to 10 is: %d\n", product);
    return NULL; 
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, calculate_product, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 58.Develop a C program to create a thread that prints the first 10 terms of the fibonacci sequence.
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t mtx;
void *range_fib(void *args)
{
        int a=0,b=1;
        for(int n=1;n<10;n++)
        {
                int c=a+b;
                a=b;
                b=c;
                pthread_mutex_lock(&mtx);
                printf("%d\n",c);
                pthread_mutex_unlock(&mtx);
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,range_fib,NULL);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 59.Develop a C program to create a thread that finds the maximum element in an array.
```c
#include<stdio.h>
#include<pthread.h>
#define SIZE 8   
int arr[SIZE] = {12, 45, 7, 89, 23, 56, 91, 34};

void *find_max(void *arg) {
    int max = arr[0];

    for (int i = 1; i < SIZE; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }

    printf("The maximum element in the array is: %d\n", max);

    return NULL;
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, find_max, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 60.Implement a C program to create a thread that prints ASCII values of characters in a given string.
```c
#include <stdio.h>
#include <pthread.h>
#include <string.h>

char str[] = "Hello";
void *print_ascii(void *arg) {
    int len = strlen(str);
    printf("String: %s\n", str);
    printf("ASCII values:\n");
    for (int i = 0; i < len; i++) {
        printf("Character: %c  ASCII: %d\n", str[i], str[i]);
    }
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, print_ascii, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 61.Develop a C program to create a thread that calculates the sum of all prime numbers up to a given limit.
```c
include <stdio.h>
#include <pthread.h>
#include <stdbool.h>
bool is_prime(int n) {
    if (n <= 1) return false;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) return false;
    }
    return true;
}
void *sum_primes(void *arg) {
    int limit = *((int *)arg);
    int sum = 0;
    for (int i = 2; i <= limit; i++) {
        if (is_prime(i)) {
            sum += i;
        }
    }
    printf("Sum of prime numbers up to %d is: %d\n", limit, sum);
    return NULL;
}

int main() {
    pthread_t t1;
    int n = 50;
    pthread_create(&t1, NULL, sum_primes, &n);
    pthread_join(t1, NULL);

    return 0;
}
```
## 62.Write a C program to create a thread that calculates the area of a circle using a given radius.
```c
#include<stdio.h>
#include<pthread.h>
#define PI 3.14
float res;
void *circle_area(void *args)
{
        int *n=(int *)args;
        res=PI*(*n)*(*n);
        return NULL;
}
int main()
{
        pthread_t t1;
        int r=5;
        pthread_create(&t1,NULL,circle_area,&r);
        pthread_join(t1,NULL);
        printf("The area of the circle is:%f\n",res);
        return 0;
}
```
## 63.Develop a C program to create a thread that calculates the average of numbers in an array?
```c
#include <stdio.h>
#include <pthread.h>

#define SIZE 6

int arr[SIZE] = {10, 20, 30, 40, 50, 60};

void *calculate_average(void *arg) {
    int sum = 0;

    for (int i = 0; i < SIZE; i++) {
        sum += arr[i];
    }

    double average = (double)sum / SIZE;
    printf("The average of array elements is: %.2f\n", average);
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, calculate_average, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 64.Implement a C program to create a thread that prints the factors of a given number.
```c
#include <stdio.h>
#include <pthread.h>

void *print_factors(void *arg) {
    int num = *((int *)arg);
    printf("Factors of %d are: ", num);
    for (int i = 1; i <= num; i++) {
        if (num % i == 0) {
            printf("%d ", i);
        }
    }
    printf("\n");
    return NULL;
}
int main() {
    pthread_t t1;
    int n = 36;
    pthread_create(&t1, NULL, print_factors, &n);
    pthread_join(t1, NULL);

    return 0;
}
```
## 65.Develop a C program to create a thread that prints the English alphabet in uppercase.
```c
#include <stdio.h>
#include <pthread.h>

void *print_uppercase(void *arg) {
    printf("Uppercase English Alphabet:\n");

    for (char c = 'A';c <= 'Z';c++) {
        printf("%c ", c);
    }
    printf("\n");
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, print_uppercase, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 66.Develop a C program that checks if a given number is divisible by another given number.
```c
#include <stdio.h>
#include <pthread.h>
struct numbers {
    int a;
    int b;
};

void *check_divisibility(void *arg) {
    struct numbers *nums = (struct numbers *)arg;

    if (nums->b == 0) {
        printf("Division by zero is not allowed!\n");
    } else if (nums->a % nums->b == 0) {
        printf("%d is divisible by %d\n", nums->a, nums->b);
    } else {
        printf("%d is NOT divisible by %d\n", nums->a, nums->b);
    }

    return NULL;
}

int main() {
    pthread_t t1;
    struct numbers nums;
    nums.a = 20;
    nums.b = 5;
    pthread_create(&t1, NULL, check_divisibility, &nums);
    pthread_join(t1, NULL);

    return 0;
}
```
## 67.Develop a C program to create a thread that prints the multiplication table of a given number up to a given limit.
```c
#include <stdio.h>
#include <pthread.h>

struct table_args {
    int num;   // the number for which table is generated
    int limit; // the limit up to which table is printed
};

void *print_table(void *arg) {
    struct table_args *t = (struct table_args *)arg;

    printf("Multiplication Table of %d up to %d:\n", t->num, t->limit);
    for (int i = 1; i <= t->limit; i++) {
        printf("%d x %d = %d\n", t->num, i, t->num * i);
    }

    return NULL;
}

int main() {
    pthread_t t1;
    struct table_args args;
    args.num = 7;
    args.limit = 10;
    pthread_create(&t1, NULL, print_table, &args);
    pthread_join(t1, NULL);

    return 0;
}
```
## 68.Develop a C program to create a thread that prints numbers from 10 to 1 in descending order using mutex locks.
```c
#include <stdio.h>
#include <pthread.h>
pthread_mutex_t lock;

void *print_descending(void *arg) {
    pthread_mutex_lock(&lock);
    printf("Numbers from 10 to 1 in descending order:\n");
    for (int i = 10; i >= 1; i--) {
        printf("%d ", i);
    }
    printf("\n");

    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_mutex_init(&lock, NULL);
    pthread_create(&t1, NULL, print_descending, NULL);
    pthread_join(t1, NULL);
    pthread_mutex_destroy(&lock);

    return 0;
}
```








