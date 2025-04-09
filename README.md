# Networking Lab - S6

## Key Network Configuration Files

### `/etc/network/interfaces`
**Purpose**: The primary file for configuring network interfaces, including IP addresses, netmasks, and gateway information.  
**Example Configuration**:  
```bash
# Static IP configuration
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4

# DHCP configuration
auto wlan0
iface wlan0 inet dhcp
```

### `/etc/resolv.conf`
**Purpose**: Stores DNS server addresses used to resolve hostnames.  
**Example Configuration**:  
```bash
nameserver 8.8.8.8
nameserver 8.8.4.4
search example.com
```

## Basic Linux Networking Commands

### `ifconfig`
**Purpose**: Displays information about network interfaces, including IP address, MAC address, and statistics.  
**Syntax**: `ifconfig [interface]`  
**Example**:  
```bash
ifconfig
ifconfig eth0
```

### `ip addr`
**Purpose**: A newer command to view and manage network interface details.  
**Syntax**: `ip addr [show] [interface]`  
**Example**:  
```bash
ip addr
ip addr show eth0
```

### `ping`
**Purpose**: Checks connectivity to a host by sending ICMP packets.  
**Syntax**: `ping [options] [destination]`  
**Example**:  
```bash
ping google.com
ping -c 4 192.168.1.1
```

### `route`
**Purpose**: Shows the routing table, including network destinations and gateways.  
**Syntax**: `route [options]`  
**Example**:  
```bash
route
route -n
```

### `netstat`
**Purpose**: Displays information about active network connections and listening ports.  
**Syntax**: `netstat [options]`  
**Example**:  
```bash
netstat -tuln
netstat -anp
```

### `nslookup`
**Purpose**: Resolves a hostname to an IP address.  
**Syntax**: `nslookup [domain]`  
**Example**:  
```bash
nslookup google.com
nslookup -type=mx gmail.com
```

### `traceroute`
**Purpose**: Traces the path a packet takes to reach a destination.  
**Syntax**: `traceroute [destination]`  
**Example**:  
```bash
traceroute google.com
traceroute -n 8.8.8.8
```

### `whois`
**Purpose**: Queries domain name and IP address registration information.  
**Syntax**: `whois [domain]`  
**Example**:  
```bash
whois example.com
whois 8.8.8.8
```

## Common Network Troubleshooting Workflow

1. Check interface status: `ifconfig` or `ip addr`
2. Verify local connectivity: `ping 127.0.0.1`
3. Test gateway connectivity: `ping <gateway-IP>`
4. Test external connectivity: `ping 8.8.8.8`
5. Test DNS resolution: `nslookup google.com`
6. Trace route to destination: `traceroute google.com`
7. Check open connections: `netstat -tuln`
8. Examine routing table: `route -n`
