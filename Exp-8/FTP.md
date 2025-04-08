# File Transfer Protocol (FTP) - A Comprehensive Guide

## Introduction to FTP

File Transfer Protocol (FTP) is a standard network protocol used for transferring files between a client and server on a computer network. Developed in the early 1970s, FTP remains one of the oldest protocols still in common use today.

## Basic Concepts

### What is FTP?

FTP is a client-server protocol that allows users to:
- Upload files from a local computer to a remote server
- Download files from a remote server to a local computer
- Manage files (rename, delete, create directories) on remote servers

### Key Characteristics

- **Connection-oriented**: Uses TCP (Transmission Control Protocol)
- **Separate control and data connections**
- **Authentication required**: Username and password (anonymous access also possible)
- **Stateful protocol**: Server maintains session information
- **Plain text transmission**: Credentials and data are not encrypted in standard FTP

## How FTP Works

### Dual Channel Architecture

FTP uses two separate channels for operation:
1. **Command/Control Channel (Port 21)**: Handles commands and responses
2. **Data Channel (Port 20 or dynamic port)**: Transfers actual file data

This separation allows for efficient command handling while data transfers are in progress.

### Connection Modes

FTP operates in two primary connection modes:

#### 1. Active Mode
- Client opens command connection to server port 21
- Client sends PORT command specifying which client port the server should connect to
- Server initiates data connection from its port 20 to client's specified port
- Problematic with firewalls as server initiates the data connection

```
+--------+                  +--------+
| Client |<-- Command ----->| Server |
|        |      (21)        |        |
|        |<-- Data ---------|        |
|        |    (20→Client)   |        |
+--------+                  +--------+
```

#### 2. Passive Mode
- Client opens command connection to server port 21
- Client sends PASV command
- Server provides IP and port for client to connect to
- Client initiates data connection to server's specified port
- Better for firewalled clients as client initiates all connections

```
+--------+                  +--------+
| Client |<-- Command ----->| Server |
|        |      (21)        |        |
|        |-----> Data ----->|        |
|        | (Client→Server)  |        |
+--------+                  +--------+
```

### FTP Session Flow

1. **Connection Establishment**: Client connects to server's port 21
2. **Authentication**: Username and password exchange
3. **Command Exchange**: Client sends commands to navigate, list directories, etc.
4. **Data Transfer Setup**: Negotiate data connection parameters
5. **Data Transfer**: File upload or download
6. **Session Termination**: Client sends QUIT command

## Common FTP Commands

| Command | Description |
|---------|-------------|
| USER    | Specify username |
| PASS    | Specify password |
| LIST    | List files and directories |
| CWD     | Change working directory |
| PWD     | Print working directory |
| RETR    | Retrieve (download) a file |
| STOR    | Store (upload) a file |
| DELE    | Delete a file |
| MKD     | Make directory |
| RMD     | Remove directory |
| PASV    | Enter passive mode |
| PORT    | Specify port for active mode |
| QUIT    | End session |

## FTP Response Codes

FTP servers respond with three-digit codes:

| Code Range | Meaning |
|------------|---------|
| 1xx        | Positive Preliminary reply |
| 2xx        | Positive Completion reply |
| 3xx        | Positive Intermediate reply |
| 4xx        | Transient Negative Completion reply |
| 5xx        | Permanent Negative Completion reply |

Common examples:
- `220`: Service ready
- `230`: User logged in
- `331`: Username OK, need password
- `425`: Can't open data connection
- `550`: Requested action not taken

## FTP Security Concerns

Standard FTP has significant security limitations:
- **No encryption**: Credentials and data are transmitted in plaintext
- **Susceptible to sniffing attacks**: Network eavesdroppers can capture credentials
- **Vulnerable to FTP bounce attacks**: Server can be tricked to connect to other servers
- **Clear text authentication**: Username and password sent unencrypted

## Secure Alternatives

### FTPS (FTP Secure)
- FTP with SSL/TLS encryption
- Same FTP commands but with encrypted channels
- Uses ports 990 (control) and 989 (data) for implicit mode
- Can use standard ports with explicit TLS (FTPES)

### SFTP (SSH File Transfer Protocol)
- Not related to traditional FTP
- File transfer protocol that uses SSH for encryption
- Single connection on port 22
- More firewall-friendly than FTPS
- Provides encryption for both authentication and data

## FTP vs. HTTP for File Transfer

| Aspect | FTP | HTTP |
|--------|-----|------|
| Primary Use | File transfer | Web browsing and API calls |
| Connections | Dual channel | Single channel |
| Stateful | Yes | No (stateless) |
| Built-in Directory Listing | Yes | No |
| Restart Capability | Yes | Limited (Range requests) |
| Firewall Friendliness | Problematic | Better |
| Authentication | Built-in | Various methods |

## Network Diagram of FTP

```
+---------------+                           +---------------+
| FTP Client    |                           | FTP Server    |
|               |                           |               |
|   +-------+   |      Control Channel      |   +-------+   |
|   |Control|<--|--(TCP Port 21)----------->|   |Control|   |
|   +-------+   |                           |   +-------+   |
|       |       |                           |       |       |
|   +-------+   |       Data Channel        |   +-------+   |
|   | Data  |<--|--(TCP Port 20 or other)-->|   | Data  |   |
|   +-------+   |                           |   +-------+   |
+---------------+                           +---------------+
```

## FTP in Modern Applications

Despite its age, FTP remains relevant in many scenarios:
- **Web hosting management**: Updating website files
- **Software distribution**: Providing downloads to users
- **Backup systems**: Transferring backup files
- **Media transfer**: Moving large audio/video files
- **Log file collection**: Centralizing logs from multiple servers

## Commonly Used FTP Clients

- **Command-line clients**: ftp, lftp
- **GUI clients**: FileZilla, WinSCP, Cyberduck
- **Browser integration**: Some browsers support FTP URLs (ftp://)
- **Programmatic clients**: Libraries in languages like Python, Java, PHP

## Viva Questions and Answers

### Q: What is FTP and what is its primary purpose?
**A:** FTP (File Transfer Protocol) is a standard network protocol used for transferring files between a client and server on a computer network. Its primary purpose is to facilitate reliable and efficient file transfers across networks.

### Q: Explain the difference between active and passive FTP modes.
**A:** In active mode, the server initiates the data connection to the client. In passive mode, the client initiates both the command and data connections to the server. Passive mode is generally more firewall-friendly since the client initiates all connections.

### Q: Why does FTP use two channels instead of one?
**A:** FTP uses separate control and data channels to allow for efficient command processing while data transfers are in progress. This separation enables simultaneous command handling and file transfers, improving efficiency.

### Q: What are the main security limitations of standard FTP?
**A:** Standard FTP transmits all data including credentials in plaintext, making it vulnerable to eavesdropping. It lacks encryption, has no protection against man-in-the-middle attacks, and can be vulnerable to bounce attacks.

### Q: How does FTPS differ from SFTP?
**A:** FTPS is FTP with added SSL/TLS encryption, maintaining the dual-channel architecture of FTP. SFTP is a completely different protocol that runs over SSH, using a single encrypted channel. SFTP is generally more firewall-friendly and considered more secure.

### Q: What ports does FTP typically use?
**A:** Standard FTP uses port 21 for the command/control channel and port 20 for the data channel in active mode. In passive mode, the data channel uses a dynamically assigned port.

### Q: What is the purpose of the LIST command in FTP?
**A:** The LIST command requests the server to send a list of files and directories in the current working directory. It's similar to the 'ls' command in Unix or 'dir' in Windows.

### Q: Explain what happens when a client connects to an FTP server.
**A:** When a client connects to an FTP server, it first establishes a control connection to port 21. The server sends a welcome message (220 response), and the client provides authentication credentials. After successful authentication, the client can send commands to navigate directories, upload, download, or manage files.

### Q: Why might someone choose SFTP over standard FTP?
**A:** SFTP offers better security through SSH encryption, protects both authentication and data transfer, uses a single connection that's easier to route through firewalls, and provides more comprehensive file operations capabilities.

### Q: What is anonymous FTP access?
**A:** Anonymous FTP allows users to connect to an FTP server without having a registered account. Users typically enter "anonymous" as the username and their email address as the password. It's commonly used for public file repositories where files are available for download without restriction.

## Conclusion

Despite being one of the oldest internet protocols, FTP continues to be widely used for file transfers. Understanding its architecture, commands, and security considerations is essential for network administrators and anyone working with file transfer systems. Modern secure variants like FTPS and SFTP address many of the original security concerns while maintaining the core functionality that has kept FTP relevant for decades.
