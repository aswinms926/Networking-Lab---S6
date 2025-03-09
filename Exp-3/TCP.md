# TCP Socket Programming in C

This repository contains a simple implementation of a TCP server and client in C. These programs demonstrate how to establish a connection and exchange messages between a server and client using socket programming.

## Contents

- [TCP Server](#tcp-server)
- [TCP Client](#tcp-client)
- [How It Works](#how-it-works)
- [Compilation and Execution](#compilation-and-execution)
- [Code Explanation](#code-explanation)
- [Complete Code](#complete-code)

## TCP Server

The server program creates a socket, binds it to a specific port, listens for incoming connections, and handles client messages.

### Key Features:

- Socket creation with error handling
- Socket reuse enabling
- Binding with detailed error reporting
- Connection acceptance
- Two-way communication with clients
- Graceful termination with "exit" command

## TCP Client

The client program connects to the server and engages in two-way communication.

### Key Features:

- Socket creation
- Connection to server (defaulting to localhost)
- Two-way message exchange
- Graceful termination with "exit" command

## How It Works

1. The server starts and waits for client connections
2. The client connects to the server
3. Both can send messages to each other
4. Either can terminate the conversation by typing "exit"

## Compilation and Execution

### Compile the server:
```bash
gcc -o server tcp_server.c
```

### Compile the client:
```bash
gcc -o client tcp_client.c
```

### Run the server:
```bash
./server
```

### Run the client:
```bash
./client
```

## Code Explanation

### TCP Server

The server includes necessary headers for networking and defines:
- `MAX`: Maximum buffer size (80 bytes)
- `PORT`: Port number (8080)
- `SA`: Shorthand for struct sockaddr

#### Communication Function

The `func()` function:
1. Clears the buffer
2. Reads client message and displays it
3. Gets server's response from user input
4. Sends response to client
5. Checks if "exit" was typed to terminate

#### Main Function

The main function:
1. Creates a socket with `socket()`
2. Enables socket reuse with `setsockopt()`
3. Sets up server address structure
4. Binds socket to address with `bind()`
5. Listens for connections with `listen()`
6. Accepts client connection with `accept()`
7. Calls the communication function
8. Closes sockets when done

### TCP Client

The client includes similar headers and defines the same constants.

#### Communication Function

The client's `func()` function:
1. Gets user input
2. Sends message to server
3. Receives and displays server's response
4. Checks for "exit" command to terminate

#### Main Function

The client's main function:
1. Creates a socket
2. Sets up server address (localhost:8080)
3. Connects to server with `connect()`
4. Calls the communication function
5. Closes the socket when done

## Error Handling

Both programs include error handling for:
- Socket creation failures
- Connection issues
- Binding problems

The server provides detailed error reporting for binding failures, indicating possible causes like port already in use.

## Notes

- This implementation uses blocking I/O which means operations wait until they complete
- The buffer size is limited to 80 bytes
- The server handles only one client at a time
- The default IP is set to 127.0.0.1 (localhost)

## Complete Code

### TCP Server Code

```c
#include <stdio.h>
#include <netdb.h>
#include <netinet/in.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <unistd.h>
#define MAX 80
#define PORT 8080
#define SA struct sockaddr

void func(int connfd)
{
    char buff[MAX];
    int n;
    for (;;) {
        bzero(buff, MAX);
        read(connfd, buff, sizeof(buff));
        printf("From client: %s\t To client : ", buff);
        
        bzero(buff, MAX);
        n = 0;
        while ((buff[n++] = getchar()) != '\n')
            ;
        
        write(connfd, buff, sizeof(buff));
        
        if (strncmp("exit", buff, 4) == 0) {
            printf("Server Exit...\n");
            break;
        }
    }
}

int main()
{
    int sockfd, connfd, len;
    struct sockaddr_in servaddr, cli;
    
    // Socket creation with error handling
    sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd == -1) {
        perror("Socket creation failed");
        exit(1);
    }
    printf("Socket successfully created..\n");
    
    // Enable socket reuse
    int enable = 1;
    if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &enable,
                sizeof(int)) < 0) {
        perror("setsockopt(SO_REUSEADDR) failed");
        exit(1);
    }
    
    // Clear server address structure
    bzero(&servaddr, sizeof(servaddr));
    
    // Assign IP and PORT
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(PORT);
    
    // Binding with detailed error handling
    if (bind(sockfd, (SA*)&servaddr, sizeof(servaddr)) != 0) {
        perror("Socket bind failed");
        printf("Error details: Possible reasons:\n");
        printf("1. Port %d might be already in use\n", PORT);
        printf("2. Insufficient permissions\n");
        printf("3. Network interface issue\n");
        close(sockfd);
        exit(1);
    }
    printf("Socket successfully binded..\n");
    
    // Listen with backlog
    if (listen(sockfd, 5) != 0) {
        perror("Listen failed");
        close(sockfd);
        exit(1);
    }
    printf("Server listening..\n");
    
    len = sizeof(cli);
    
    // Accept connection
    connfd = accept(sockfd, (SA*)&cli, &len);
    if (connfd < 0) {
        perror("Server accept failed");
        close(sockfd);
        exit(1);
    }
    printf("Server accepted the client...\n");
    
    // Chat function
    func(connfd);
    
    // Close sockets
    close(connfd);
    close(sockfd);
    
    return 0;
}
```

### TCP Client Code

```c
#include <arpa/inet.h> // inet_addr()
#include <netdb.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <strings.h> // bzero()
#include <sys/socket.h>
#include <unistd.h> // read(), write(), close()
#define MAX 80
#define PORT 8080
#define SA struct sockaddr

void func(int sockfd)
{
    char buff[MAX];
    int n;
    for (;;) {
        bzero(buff, sizeof(buff));
        printf("Enter the string : ");
        n = 0;
        while ((buff[n++] = getchar()) != '\n')
            ;
        write(sockfd, buff, sizeof(buff));
        bzero(buff, sizeof(buff));
        read(sockfd, buff, sizeof(buff));
        printf("From Server : %s", buff);
        if ((strncmp(buff, "exit", 4)) == 0) {
            printf("Client Exit...\n");
            break;
        }
    }
}

int main()
{
    int sockfd, connfd;
    struct sockaddr_in servaddr, cli;
    
    // socket create and verification
    sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd == -1) {
        printf("socket creation failed...\n");
        exit(0);
    }
    else
        printf("Socket successfully created..\n");
    bzero(&servaddr, sizeof(servaddr));
    
    // assign IP, PORT
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = inet_addr("127.0.0.1");
    servaddr.sin_port = htons(PORT);
    
    // connect the client socket to server socket
    if (connect(sockfd, (SA*)&servaddr, sizeof(servaddr))
        != 0) {
        printf("connection with the server failed...\n");
        exit(0);
    }
    else
        printf("connected to the server..\n");
    
    // function for chat
    func(sockfd);
    
    // close the socket
    close(sockfd);
}
```
