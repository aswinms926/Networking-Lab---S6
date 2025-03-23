# Leaky Bucket Algorithm Implementation

This C program demonstrates the implementation of the Leaky Bucket algorithm, a traffic shaping mechanism used in network congestion control.

## Algorithm Overview

The Leaky Bucket algorithm is used to control the rate at which packets are sent to the network, preventing network congestion by:

1. Temporarily storing packets in a buffer (bucket)
2. Forwarding them at a constant rate
3. Dropping excess packets when the buffer overflows

## Program Description

This implementation simulates a leaky bucket with:
- A configurable bucket size
- A constant outgoing rate
- A variable incoming packet rate

## Features

- User-defined bucket size
- User-defined outgoing packet rate
- Simulation of incoming packets of different sizes
- Visual representation of buffer usage
- Overflow handling with packet dropping

## Code Breakdown

```c
#include<stdio.h>

int main() {
    int incoming, outgoing, buck_size, n, store = 0;
    
    // Getting input parameters
    printf("Enter bucket size, outgoing rate and no of inputs : ");
    scanf("%d %d %d", &buck_size, &outgoing, &n);
    
    // Processing each incoming packet
    while (n != 0) {
        printf("Enter the incoming packet size : ");
        scanf("%d", &incoming);
        printf("Incoming packet size %d\n", incoming);
        
        // Checking if the packet fits in the buffer
        if (incoming <= (buck_size - store)) {
            store += incoming;
            printf("Bucket buffer size :  %d out of %d\n", store, buck_size);
        } else {
            // Handling overflow
            printf("Dropped %d no of packets\n", incoming - (buck_size - store));
            store = buck_size;
            printf("Bucket buffer size :  %d out of %d\n", store, buck_size);
        }
        
        // Processing outgoing packets
        if(store >= outgoing) {
            store = outgoing - store;
        }
        
        if(store < 0) {
            store = store * (-1);
        }
        
        printf("After outgoing, There are %d out of %d packets left in buffer\n", store, buck_size);
        n--;
    }
}
```

## How to Use

1. Compile the program:
   ```
   gcc leaky_bucket.c -o leaky_bucket
   ```

2. Run the executable:
   ```
   ./leaky_bucket
   ```

3. Enter the requested parameters:
   - Bucket size (maximum buffer capacity)
   - Outgoing rate (packets that can be forwarded per cycle)
   - Number of input operations to simulate

4. For each cycle, enter the incoming packet size when prompted

## Sample Output

```
Enter bucket size, outgoing rate and no of inputs : 10 3 3
Enter the incoming packet size : 4
Incoming packet size 4
Bucket buffer size :  4 out of 10
After outgoing, There are 1 out of 10 packets left in buffer
Enter the incoming packet size : 7
Incoming packet size 7
Bucket buffer size :  8 out of 10
After outgoing, There are 5 out of 10 packets left in buffer
Enter the incoming packet size : 8
Incoming packet size 8
Dropped 3 no of packets
Bucket buffer size :  10 out of 10
After outgoing, There are 7 out of 10 packets left in buffer
```

## Educational Notes

1. The leaky bucket algorithm ensures a consistent outflow rate, even when incoming packet rates vary.
2. This implementation demonstrates how packet dropping works when the buffer is full.
3. In real networks, this algorithm helps prevent network congestion and ensures fair bandwidth allocation.

## Possible Improvements

- Add monitoring of packet loss percentage
- Implement time-based simulation instead of discrete steps
- Add graphical representation of buffer usage
- Implement variable outgoing rates

## Applications

- Network traffic shaping
- Quality of Service (QoS) implementation
- Congestion control in routers
- Rate limiting in APIs
