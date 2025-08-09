# TCP and UDP Terms and Common Network Ports

## Overview
This README.md file explains what TCP and UDP terms mean, how they are different, and lists the most commonly used ports in web services (HTTP, HTTPS, SSH, Telnet, FTP, SFTP).

## Table of Contents
1. [What is TCP?](#what-is-tcp)
2. [What is UDP?](#what-is-udp)
3. [Key Differences Between TCP and UDP](#key-differences-between-tcp-and-udp)
4. [TCP vs UDP Comparison](#tcp-vs-udp-comparison)
5. [When to Use TCP vs UDP](#when-to-use-tcp-vs-udp)
6. [Common Network Ports](#common-network-ports)
7. [Detailed Port Information](#detailed-port-information)
8. [Port Categories](#port-categories)
9. [Security Considerations](#security-considerations)
10. [Conclusion](#conclusion)

## What is TCP?

**TCP (Transmission Control Protocol)** is a connection-oriented protocol that provides reliable, ordered, and error-checked delivery of data between applications running on hosts communicating over an IP network.

### Key Characteristics of TCP:

**Connection-Oriented**: TCP is a connection-based protocol that establishes a connection before data transfer. It uses a three-way handshake process to establish connections between sender and receiver.

**Reliable Data Transfer**: TCP ensures that all data sent is received correctly and in the proper order. If packets are lost or corrupted during transmission, TCP automatically retransmits them.

**Error Detection and Correction**: TCP includes mechanisms to detect errors in transmitted data and request retransmission of corrupted packets.

**Flow Control**: TCP manages the rate of data transmission between sender and receiver to prevent overwhelming the receiving device.

**Ordered Data Delivery**: Data arrives at the destination in the same order it was sent from the source.

### How TCP Works:

1. **Connection Establishment**: Three-way handshake (SYN → SYN-ACK → ACK)
2. **Data Transfer**: Reliable transmission with acknowledgments
3. **Connection Termination**: Four-way handshake to close the connection

## What is UDP?

**UDP (User Datagram Protocol)** is a connectionless protocol that provides a simple, fast method for sending data without establishing a formal connection or guaranteeing delivery.

### Key Characteristics of UDP:

**Connectionless**: UDP is connectionless, so it doesn't establish a prior connection and doesn't require a three-way handshake like TCP.

**Unreliable but Fast**: While TCP is more reliable, it transfers data more slowly. UDP is less reliable but works more quickly.

**No Error Recovery**: UDP throws out all the error-checking stuff, all the back-and-forth communication and deliverability.

**Lightweight**: UDP has minimal protocol overhead compared to TCP.

**No Guaranteed Delivery**: UDP doesn't guarantee that all data is successfully transferred and sends data to any device that happens to be listening.

### How UDP Works:

1. **Data Transmission**: Direct sending of data packets (datagrams)
2. **No Connection Setup**: Immediate data transfer without handshake
3. **No Acknowledgments**: No confirmation of successful delivery

## Key Differences Between TCP and UDP

### Primary Differences:

**Connection Type**:
- TCP is connection oriented – once a connection is established, data can be sent bidirectional
- UDP is a simpler, connectionless protocol

**Reliability**:
- TCP ensures reliable, ordered data transmission, which is crucial for applications prioritising data integrity
- UDP prioritises speed and efficiency, accepting occasional packet loss

**Speed**:
- TCP is more dependable, but it moves data more slowly
- Although less dependable, UDP operates more quickly

**Use Cases**:
- Because of its reliability, protocols like HTTP, FTP, etc., use TCP for secured data transmission over the network
- UDP is often used in situations where higher speeds are crucial, like in streaming or gaming

## TCP vs UDP Comparison

| Feature | TCP | UDP |
|---------|-----|-----|
| **Connection Type** | Connection-oriented | Connectionless |
| **Reliability** | Reliable delivery | Unreliable delivery |
| **Speed** | Slower due to overhead | Faster with minimal overhead |
| **Error Checking** | Extensive error checking | Basic error checking |
| **Data Ordering** | Guarantees order | No order guarantee |
| **Flow Control** | Yes | No |
| **Congestion Control** | Yes | No |
| **Header Size** | 20-60 bytes | 8 bytes |
| **Retransmission** | Yes, automatic | No |
| **Broadcasting** | Not supported | Supported |

## When to Use TCP vs UDP

### Use TCP When:
- **Data integrity is critical** (file transfers, web browsing, email)
- **All data must arrive** and in correct order
- **Connection-based communication** is needed
- **Error recovery** is important
- Examples: HTTP/HTTPS, FTP, SMTP, SSH

### Use UDP When:
- **Speed is more important than reliability** (gaming, streaming)
- **Real-time applications** where delayed data is useless
- **Broadcasting or multicasting** is required
- **Low overhead** is needed
- Examples: DNS, DHCP, video streaming, online gaming

## Common Network Ports

Based on research, here are the most commonly used network ports for web services:

### Essential Web Service Ports:

| Service | Port Number | Protocol | Description |
|---------|------------|----------|-------------|
| **HTTP** | 80 | TCP | Hypertext Transfer Protocol - Web traffic |
| **HTTPS** | 443 | TCP | Secure HTTP - Encrypted web traffic |
| **SSH** | 22 | TCP | Secure Shell - Remote login and file transfer |
| **Telnet** | 23 | TCP | Remote login (unencrypted) |
| **FTP** | 20, 21 | TCP | File Transfer Protocol |
| **SFTP** | 22 | TCP | Secure FTP (uses SSH) |

### Additional Common Ports:

| Service | Port Number | Protocol | Description |
|---------|------------|----------|-------------|
| **DNS** | 53 | UDP/TCP | Domain Name System |
| **SMTP** | 25 | TCP | Simple Mail Transfer Protocol |
| **POP3** | 110 | TCP | Post Office Protocol v3 |
| **IMAP** | 143 | TCP | Internet Message Access Protocol |
| **SNMP** | 161 | UDP | Simple Network Management Protocol |
| **DHCP** | 67, 68 | UDP | Dynamic Host Configuration Protocol |

## Detailed Port Information

### HTTP - Port 80
- **Protocol**: TCP
- **Purpose**: Standard web traffic and HTTP communication
- **Security**: Unencrypted
- **Common Use**: Regular websites, web applications

### HTTPS - Port 443
- **Protocol**: TCP
- **Purpose**: Secure web traffic with SSL/TLS encryption
- **Security**: Encrypted
- **Common Use**: Secure websites, online banking, e-commerce

### SSH - Port 22
- **Protocol**: TCP
- **Purpose**: Secure remote login and secure file transfers
- **Security**: Encrypted
- **Common Use**: Server administration, secure file transfer (SFTP)

### Telnet - Port 23
- **Protocol**: TCP
- **Purpose**: Remote login services allowing users to access and manage remote systems
- **Security**: Unencrypted (largely replaced by SSH)
- **Common Use**: Legacy system access (deprecated)

### FTP - Ports 20 and 21
- **Protocol**: TCP
- **Purpose**: File transfer between systems
- **Port 21**: Control connection (commands)
- **Port 20**: Data connection (file transfer)
- **Security**: Unencrypted
- **Common Use**: File sharing, website uploads

### SFTP - Port 22
- **Protocol**: TCP (uses SSH)
- **Purpose**: Secure file transfer protocol
- **Security**: Encrypted via SSH tunnel
- **Common Use**: Secure file transfers, modern replacement for FTP

## Port Categories

### Well-Known Ports (0-1023)
The port numbers in the range from 0 to 1023 are the well-known System Ports assigned by the Internet Assigned Numbers Authority:

- **System/Privileged Ports**: Require administrative privileges
- **Standardized Services**: Official protocol assignments
- **Examples**: HTTP (80), HTTPS (443), SSH (22), FTP (21)

### User Ports (1024-49151)
User Ports range from 1024-49151:

- **Registered Ports**: Semi-reserved for specific applications
- **Custom Applications**: User-defined services
- **Examples**: MySQL (3306), RDP (3389)

### Dynamic/Private Ports (49152-65535)
Dynamic and/or Private Ports range from 49152-65535:

- **Ephemeral Ports**: Temporary ports for client connections
- **Private Use**: Not officially assigned
- **Automatic Assignment**: Used by operating systems

## Security Considerations

### Port Security Best Practices:

**1. Close Unused Ports**
- Disable services not in use
- Reduce attack surface
- Regular port scanning audits

**2. Use Secure Alternatives**
- SSH instead of Telnet
- SFTP instead of FTP
- HTTPS instead of HTTP

**3. Firewall Configuration**
- Block unnecessary incoming ports
- Allow only required services
- Monitor port usage

**4. Regular Updates**
- Keep services updated
- Apply security patches
- Monitor for vulnerabilities

### Common Security Risks:

**Open Ports**: Unnecessary open ports provide attack vectors
**Default Configurations**: Default settings often lack proper security
**Unencrypted Protocols**: Telnet and FTP transmit data in plain text
**Port Scanning**: Attackers use port scans to identify services

## Conclusion

Understanding TCP and UDP protocols and common network ports is fundamental for web development, network administration, and cybersecurity:

### Key Takeaways:

**Protocol Selection:**
- TCP is connection-based and more reliable but slower
- UDP is connectionless, less reliable but faster
- Choose based on application requirements for reliability vs. speed

**Common Ports Knowledge:**
- Ports 80, 443, 20, 21, 22, 23, 25, and 53 are most commonly encountered
- Well-known ports (0-1023) are assigned by Internet authorities
- Understanding port functions improves troubleshooting and security

**Security Implications:**
- Use encrypted protocols (HTTPS, SSH, SFTP) over unencrypted ones
- Proper port management is crucial for network security
- Regular monitoring and updates prevent vulnerabilities

**Practical Applications:**
- Web developers need HTTP/HTTPS knowledge
- System administrators require SSH/FTP understanding
- Network engineers must know all common port assignments

This knowledge forms the foundation for network communication, web development, and cybersecurity practices in modern computing environments.

---
