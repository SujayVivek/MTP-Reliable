MTP: Reliable Message-Oriented Transport Protocol over UDP
Overview
This project implements MTP (My Transport Protocol), a custom, message-oriented, reliable data transfer protocol built on top of UDP sockets. MTP provides in-order, lossless delivery of fixed-size messages using window-based flow control, custom acknowledgment, retransmission, and a shared memory management architecture. The solution is designed to function as a drop-in replacement for UDP in user applications, exposing familiar socket-like APIs.

Features
Reliable, in-order delivery of 1KB messages over unreliable UDP

Sliding window protocol with sender (swnd) and receiver (rwnd) window management

Custom ACK handling with piggybacked window size and last in-order sequence number

Automatic retransmission on timeout (configurable, default T=5s)

Duplicate detection and handling for both data and ACK messages

Dynamic window resizing based on receiver buffer state

Support for up to 25 concurrent MTP sockets

Shared memory architecture for socket state, buffers, and window management

Multi-threaded design: separate threads for sending, receiving, and garbage collection

Simulated packet loss via dropMessage(p) for robust testing

Static library interface (libmsocket.a) for easy integration

File Structure
msocket.h, msocket.c — MTP socket API and protocol implementation

initmsocket.c — Initialization of threads, shared memory, and garbage collector

user1.c, user2.c — Sample applications for file transfer using MTP

Makefile — Build static library and executables

documentation.txt — Data structures, function descriptions, and test results

Usage
1. Build the Library and Executables
bash
make           # Builds libmsocket.a
make init      # Builds the initmsocket executable
make user1     # Builds user1 (sender) executable
make user2     # Builds user2 (receiver) executable
2. Run the Protocol
Start the initializer:

bash
./initmsocket
In separate terminals, run:

bash
./user1 <local_ip> <local_port> <remote_ip> <remote_port> <input_file>
./user2 <local_ip> <local_port> <remote_ip> <remote_port> <output_file>
3. Testing with Simulated Loss
Adjust the packet loss probability p in msocket.h (e.g., #define P 0.1)

Test with different values of p (0.05 to 0.5) and record transmission statistics as required

API
The following functions are provided (mirroring UDP socket APIs):

int m_socket(int domain, int type, int protocol);

int m_bind(int sockfd, struct sockaddr *addr, socklen_t addrlen, struct sockaddr *dest_addr, socklen_t dest_len);

ssize_t m_sendto(int sockfd, const void *buf, size_t len, int flags, struct sockaddr *dest_addr, socklen_t addrlen);

ssize_t m_recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr *src_addr, socklen_t *addrlen);

int m_close(int sockfd);

Additional:

int dropMessage(float p); — Simulates packet loss for testing

How It Works
Sender buffers up to 10 messages, maintains a sliding window (default size 5), and retransmits on timeout.

Receiver buffers up to 5 messages, delivers only in-order messages to the application, and sends ACKs with window updates.

Shared memory tracks all socket state, buffers, and window info for up to 25 sockets.

Thread R handles incoming UDP messages, buffer management, and ACKs.

Thread S manages timeouts, retransmissions, and sending pending messages.

Garbage collector cleans up orphaned sockets and shared memory.

Packet loss is simulated using dropMessage(p) for robust protocol validation.

Documentation
See documentation.txt for:

All data structures and their fields

Function descriptions for msocket.c and initmsocket.c

Test results table: average transmissions per message for various p values

Notes
Message size is fixed at 1KB.

Sequence numbers are 4 bits (wrap at 16).

All buffers and window sizes are configurable in msocket.h.

Non-blocking send/receive semantics.

Designed for simplicity and extensibility.

License
This project is for academic and educational use.

Contact: For questions or issues, please open a GitHub issue or contact the repository maintainer.
