# UDP Socket Communication

## Client Code

```c
#include <arpa/inet.h>
#include <netdb.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <unistd.h>

int main(int argc, char *argv[])
{
    struct sockaddr_in serveraddr;
    
    // Check command-line arguments
    if (argc != 3)
    {
        printf("Usage: %s <server_ip> <port>\n", argv[0]);
        return 1;
    }
    
    // Create UDP socket
    int sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    if (sockfd == -1)
    {
        printf("Error in socket creation\n");
        return 1;
    }
    
    // Configure server address
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = inet_addr(argv[1]);
    serveraddr.sin_port = htons(atoi(argv[2]));
    
    // Prepare message buffer
    char buff[100];
    printf("Message to server: ");
    fgets(buff, sizeof(buff), stdin);
    
    // Send message to server
    sendto(sockfd, buff, strlen(buff), 0, 
           (struct sockaddr *)&serveraddr, sizeof(serveraddr));
    
    printf("Message sent to server!\n");
    close(sockfd);
    return 0;
}
```

## server code
```
#include <arpa/inet.h>
#include <netdb.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <unistd.h>

int main(int argc, char *argv[])
{
    struct sockaddr_in serveaddr, cli;
    
    // Check command-line arguments
    if (argc != 2)
    {
        printf("Usage: %s <port>\n", argv[0]);
        return 1;
    }
    
    // Create UDP socket
    int sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    if (sockfd == -1)
    {
        printf("Error in socket creation\n");
        return 1;
    }
    
    // Configure server address
    serveaddr.sin_family = AF_INET;
    serveaddr.sin_addr.s_addr = INADDR_ANY;
    serveaddr.sin_port = htons(atoi(argv[1]));
    
    // Bind socket to address
    if (bind(sockfd, (struct sockaddr *)&serveaddr, sizeof(serveaddr)) < 0)
    {
        printf("Error in binding socket\n");
        exit(1);
    }
    
    // Prepare to receive message
    char buff[100];
    socklen_t len = sizeof(cli);
    
    printf("Server waiting for message...\n");
    
    // Receive message from client
    if (recvfrom(sockfd, buff, sizeof(buff), 0, 
                 (struct sockaddr *)&cli, &len) < 0)
    {
        printf("Error in receiving message\n");
        return 1;
    }
    
    // Display received message
    printf("Received from client: %s\n", buff);
    
    close(sockfd);
    return 0;
}
```

# Compile client
```gcc client.c -o client```

# Compile server
```gcc server.c -o server```

# Run server (first)
```./server 8080```

# Run client (in another terminal)
```./client 127.0.0.1 8080```

1. UDP Socket Communication
   - Uses User Datagram Protocol (UDP)
   - Connectionless communication
   - Lightweight and fast

2. Socket Programming Steps
   - Create socket
   - Configure address
   - Bind (server)
   - Send/Receive messages

3. Key Functions
   - socket(): Create communication endpoint
   - sendto(): Send message
   - recvfrom(): Receive message
   - bind(): Attach socket to local address
