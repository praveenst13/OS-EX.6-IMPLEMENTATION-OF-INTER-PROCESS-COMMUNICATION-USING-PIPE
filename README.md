# OS-EX.6-IMPLEMENTATION-OF-INTER-PROCESS-COMMUNICATION-USING-PIPE

## AIM:
To implement interprocess communication using pipe command.

## ALGORITHM:
  1.Start
  
  2.Create a child and parent using fork()
  
  3.Allow communication between both the process
  
  4.Stop

## PROGRAM:


```
#include <stdio.h>
#include <unistd.h> // Include this header for fork() and pipe()

int main()
{
    int p[2], pid, pid1;
    char msg[25], msg1[25];
    pipe(p);
    pid = fork();

    if (pid != 0) {
        sleep(2);
        read(p[0], msg1, 21);
        printf("%s\n", msg1);
    } else {
        pid1 = fork();
        if (pid1 != 0) {
            sleep(1);
            write(p[1], "Says hello to grandpa", 21);
        } else {
            write(p[1], "Grand child says hello", 22);
        }
    }

    return 0;
}
```

### INTERPROCESS COMMUNICATION SHARED MEMORY:


```                              
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<wait.h>
int main(void)
{
  int shmid;
  char *shmPtr;
  int n;
  if (fork( ) == 0)
  {
     sleep(5); /* UUPS */
     if( (shmid = shmget(2041, 32, 0)) == -1 )
     {
        exit(1);
     }
     shmPtr = shmat(shmid, 0, 0);
     if (shmPtr == (char *) -1)
        exit(2);
     printf ("\nChild Reading ....\n\n");
     for (n = 0; n < 26; n++)
        putchar(shmPtr[n]);
        putchar('\n'); }
  else
  {
     if( (shmid = shmget(2041, 32, 0666 | IPC_CREAT)) == -1 )
     {
        exit(1);
     }

     shmPtr = shmat(shmid, 0, 0);
     if (shmPtr == (char *) -1)
        exit(2);
     for (n = 0; n < 26; n++)
     shmPtr[n] = 'a' + n;
     printf ("Parent Writing ....\n\n") ;
     for (n = 0; n < 26; n++)
     putchar(shmPtr[n]);
     putchar('\n');
     wait(NULL);
     shmdt(NULL);
     if( shmctl(shmid, IPC_RMID, NULL) == -1 )
     {
        perror("shmctl");
        exit(-1);
     }
  }
     exit(0);
}

```



## OUTPUT:


![image](https://github.com/praveenst13/OS-EX.6-IMPLEMENTATION-OF-INTER-PROCESS-COMMUNICATION-USING-PIPE/assets/118787793/6fa64417-1a81-4ca0-bd5c-ef537041d9be)
### INTERPROCESS COMMUNICATION SHARED MEMORY:
![image](https://github.com/praveenst13/OS-EX.6-IMPLEMENTATION-OF-INTER-PROCESS-COMMUNICATION-USING-PIPE/assets/118787793/a82a7170-051c-4246-95e9-1efd15227406)




## RESULT:
