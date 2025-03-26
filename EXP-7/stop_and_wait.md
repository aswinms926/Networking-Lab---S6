# Selective Repeat ARQ (Automatic Repeat reQuest) Simulation

## Code Implementation

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>  // Required for srand()

// Frame structure to represent network packets
struct frame {
    int info;       // Information/data in the frame
    int seq;        // Sequence number of the frame
};

// Global variables
int ak, t = 5, k;
int disconnected = 0;
struct frame p;
int errorack = 1;
int errorframe = 1;
char turn = 's';

// Function prototypes
void sender();
void receiver();

int main() {
    srand(time(NULL)); // Initialize random seed for rand()
    
    p.info = 0;
    p.seq = 0;
    while (!disconnected) {
        sender();
        for (k = 0; k < 10000000; k++); // Delay simulation
        receiver();
    }
    return 0;
}

void sender() {
    static int flag = 0;
    if (turn == 's') {
        if (errorack == 0) {
            printf("SENDER: Sent packet with seq no %d\n", p.seq);
            errorframe = rand() % 4;
            if (errorframe == 0) {
                printf("SENDER: Error while sending packet\n");
            }
            turn = 'r';
        } else {
            if (flag == 1) {
                printf("SENDER: Received ACK for packet %d\n", ak);
            }
            if (p.seq == 5) {
                disconnected = 1;
                return;
            }
            p.info += 1;
            p.seq += 1;
            printf("SENDER: Sent packet with seq no %d\n", p.seq);
            errorframe = rand() % 4;
            if (errorframe == 0) {
                printf("SENDER: Error while sending packet\n");
            }
            turn = 'r';
            flag = 1;
        }
    } else {
        t--;
        if (t == 0) {
            errorack = 0;
            turn = 's';
            t = 5;
        }
    }
}

void receiver() {
    static int f = 1;
    if (turn == 'r') {
        if (errorframe != 0) {
            if (p.seq == f) {
                printf("RECEIVER: Received packet with seq no %d\n", p.seq);
                ak = p.seq;
                f += 1;
                turn = 's';
                errorack = rand() % 4;
                if (errorack == 0) {
                    printf("RECEIVER: Error while sending ACK\n");
                }
            } else {
                printf("RECEIVER: Duplicate packet with seq no %d\n", f - 1);
                ak = f - 1;  // Corrected duplicate handling
                turn = 's';
                errorack = rand() % 4;
                if (errorack == 0) {
                    printf("RECEIVER: Error while sending ACK\n");
                }
            }
        }
    }
}
```
## Protocol Overview

Selective Repeat ARQ (Automatic Repeat reQuest) is a data transmission protocol designed to:
- Ensure reliable data transmission
- Handle packet loss and errors
- Implement sliding window mechanism for efficient communication

## Key Components

### 1. Frame Structure
- Represents a network packet
- Contains two primary elements:
  * `info`: Payload data
  * `seq`: Sequence number for packet tracking

### 2. Error Simulation Mechanism
- Uses random number generation to simulate network errors
- Randomly introduces:
  * Packet transmission errors
  * Acknowledgement errors
- Provides realistic network communication scenario

### 3. Communication Flow
- Alternating sender and receiver turns
- Controlled by `turn` variable
- Implements basic packet exchange protocol

## Simulation Characteristics

### Packet Transmission
- Sends maximum of 5 packets
- Simulates real-world network communication
- Introduces artificial delays between transmissions

### Error Handling
- Detects and logs:
  * Packet transmission errors
  * Duplicate packet reception
  * Acknowledgement failures

## Implementation Details

### Main Simulation Loop
- Continuous packet exchange
- Terminates after 5 successful packet transmissions
- Uses random seed for error generation

### Sender Function
- Manages packet transmission
- Handles acknowledgement scenarios
- Tracks packet sequence

### Receiver Function
- Receives and validates packets
- Manages acknowledgement process
- Detects duplicate packets

## Compilation Instructions

```bash
gcc -o arq_simulation arq_simulation.c
./arq_simulation
```
