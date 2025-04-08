# Understanding Wireshark Tool

## Experiment Objectives

This experiment aims to:

1. Capture and analyze network packets while browsing a website using Wireshark
2. Investigate protocols, header field values, and packet sizes
3. Explore key Wireshark features including:
   - Filters
   - TCP flow graphs
   - Statistics
   - Protocol hierarchy

## Introduction

Wireshark is a powerful, cross-platform network packet analyzer used by network engineers worldwide. It captures packets transmitted through a network interface and displays them in a formatted, readable manner for analysis.

### What is Wireshark?

Wireshark is an open-source, free network protocol analyzer that allows users to examine data from a live network or from a capture file on disk. It provides a comprehensive view of network traffic by:

- Capturing raw packet data from network interfaces
- Allowing deep inspection of hundreds of protocols
- Providing powerful filtering capabilities
- Supporting real-time capture and offline analysis
- Offering multi-platform support (Windows, macOS, Linux)
- Featuring a rich GUI interface and command-line utilities like TShark

Originally named Ethereal, Wireshark has become the industry standard tool for network troubleshooting, analysis, software development, and security auditing. Network professionals use it to identify network problems, security vulnerabilities, and analyze protocol implementations.

### Common Use Cases

- **Network Troubleshooting**: Diagnose network issues and latency problems
- **Security Analysis**: Detect suspicious traffic patterns and potential security breaches
- **Protocol Development**: Test and debug new network protocols
- **Network Learning**: Study how protocols work in practical implementations
- **Performance Optimization**: Identify bandwidth usage patterns and optimize network performance
- **Forensic Analysis**: Investigate network-related incidents

## Installation Process (Ubuntu)

### Step 1: Update Package Repository
```bash
sudo apt update
```

### Step 2: Install Wireshark
```bash
sudo apt install wireshark
```
Press 'y' when prompted to continue with installation.

### Step 3: Configure User Permissions
When prompted during installation, select "Yes" to allow non-root users to capture packets.

To add your user account to the Wireshark group (allows running without root privileges):
```bash
sudo usermod -aG wireshark $(whoami)
```

### Step 4: Reboot System
```bash
sudo reboot
```

## Launching Wireshark

You can start Wireshark in two ways:
1. From the Application Menu in Ubuntu
2. From the Terminal:
```bash
wireshark
```

## Packet Capture and Analysis

Using Wireshark, we can:
1. Capture packets during web browsing
2. Analyze protocols used in packet transmission
3. Examine header fields and packet sizes
4. Visualize network traffic patterns

### Capture Interface Selection

When Wireshark launches, it displays available network interfaces. Select the appropriate interface (typically your Ethernet or Wi-Fi adapter) to begin capturing packets. Common interfaces include:

- **eth0/ens33**: Ethernet interface
- **wlan0/wlp2s0**: Wi-Fi interface
- **lo**: Loopback interface (for local traffic)

### Understanding the Wireshark Interface

The Wireshark interface consists of several key components:

1. **Packet List Pane**: Shows a summary of each captured packet
2. **Packet Details Pane**: Displays the protocols and protocol fields for the selected packet
3. **Packet Bytes Pane**: Shows the raw data of the packet in hexadecimal and ASCII
4. **Display Filter Bar**: Allows filtering of displayed packets based on specific criteria

## Feature Exploration

Wireshark provides several powerful features for network analysis:

### 1. Filters

Wireshark offers two types of filters:
- **Capture Filters**: Applied before packets are captured, reducing the amount of data collected
- **Display Filters**: Applied to already captured packets to focus analysis

Common filter examples:
```
# Display only HTTP traffic
http

# Display traffic to/from a specific IP address
ip.addr == 192.168.1.1

# Display TCP traffic on port 80
tcp.port == 80

# Display all DNS traffic
dns

# Display packets with errors
http.response.code >= 400
```

### 2. TCP Flow Graphs

TCP flow graphs visualize the exchange of packets between hosts:
- Access through: Statistics → Flow Graph
- Shows the sequence of TCP packets exchanged in a connection
- Helps identify connection issues, retransmissions, and delays

### 3. Statistics

Wireshark provides numerous statistical tools:
- **Protocol Hierarchy**: Shows the distribution of protocols in the capture
- **Conversations**: Lists all connections between hosts
- **Endpoints**: Shows all active hosts in the capture
- **I/O Graphs**: Visualizes traffic patterns over time
- **Service Response Time**: Analyzes response times for various services

### 4. Protocol Hierarchy

The protocol hierarchy view:
- Provides a tree view of all protocols detected in the capture
- Shows the percentage of packets using each protocol
- Helps identify the dominant protocols in the network traffic
- Access through: Statistics → Protocol Hierarchy

## Conclusion

This experiment demonstrates how to use Wireshark to capture and analyze network traffic, providing insights into network protocols, packet structures, and communication patterns during web browsing. Wireshark's comprehensive feature set makes it an invaluable tool for network professionals to diagnose issues, optimize performance, and understand complex network interactions.
