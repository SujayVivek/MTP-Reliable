# 🛰️ MTP: Reliable Message-Oriented Transport Protocol over UDP

## 🚀 Overview

**MTP (My Transport Protocol)** is a custom, message-oriented, reliable data transfer protocol built on top of UDP. This project implements a drop-in replacement for UDP sockets, guaranteeing in-order, lossless delivery of fixed-size messages using a window-based flow control mechanism. Designed for performance and reliability, MTP exposes a familiar socket-like API for easy integration into user applications.

---

## ✨ Features

- **Reliable, In-Order Delivery:** Ensures 1KB messages always arrive in sequence, even over unreliable UDP.
- **Sliding Window Protocol:** Sender and receiver maintain dynamic windows for efficient flow control.
- **Custom ACK Handling:** Piggybacks window size and last in-order sequence number for smart feedback.
- **Automatic Retransmission:** Messages are resent on timeout (default 5s), minimizing loss.
- **Duplicate Detection:** Identifies and drops duplicate data and ACKs.
- **Dynamic Window Resizing:** Adapts to buffer state for optimal throughput.
- **Multi-Socket Support:** Up to 25 concurrent MTP sockets.
- **Shared Memory Architecture:** Centralized state and buffer management.
- **Multithreaded Design:** Separate threads for sending, receiving, and garbage collection.
- **Simulated Packet Loss:** `dropMessage(p)` for robust testing.
- **Static Library:** Easy linking via `libmsocket.a`.

---

## 🗂️ File Structure

| File                | Purpose                                         |
|---------------------|-------------------------------------------------|
| `msocket.h/.c`      | MTP socket API and protocol implementation      |
| `initmsocket.c`     | Initializes threads, shared memory, GC process  |
| `user1.c`           | Sender demo application                         |
| `user2.c`           | Receiver demo application                       |
| `Makefile`          | Build library and executables                   |
| `documentation.txt` | Data structures, function docs, test results    |

---

## ⚡ Getting Started

### 1. Build Everything


### 2. Run the Protocol

- **Start the initializer:**
- **In separate terminals, run:**


### 3. Simulate Packet Loss

- Set the packet loss probability `p` in `msocket.h` (e.g., `#define P 0.1`)
- Test with different `p` values (0.05 to 0.5) and record transmission stats

---

## 🛠️ API


---

## 🔎 How It Works

- **Sender:** Buffers up to 10 messages, manages a sliding window (default 5), retransmits on timeout.
- **Receiver:** Buffers up to 5 messages, delivers only in-order data, sends ACKs with window updates.
- **Shared Memory:** Tracks all socket state, buffers, window info for up to 25 sockets.
- **Thread R:** Handles incoming UDP, buffer management, and ACKs.
- **Thread S:** Manages timeouts, retransmissions, and sending.
- **Garbage Collector:** Cleans up orphaned sockets and memory.
- **Packet Loss:** Simulated via `dropMessage(p)` for robust protocol testing.

---

## 📄 Documentation

See `documentation.txt` for:
- Data structures and field descriptions
- Function documentation for `msocket.c` and `initmsocket.c`
- Test results table: avg. transmissions per message for various `p` values

---

## ⚠️ Notes

- **Message size:** Fixed at 1KB
- **Sequence numbers:** 4 bits (wrap at 16)
- **Buffers:** Configurable in `msocket.h`
- **Non-blocking:** Send/receive semantics
- **Designed for:** Simplicity, extensibility, and robust testing

---

## 📝 License

For academic and educational use only.

---

**Questions?** Open a GitHub issue or contact the maintainer.
