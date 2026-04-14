# Multi-Container Runtime with Supervisor and Memory Monitor

## 1. Team Information

* Name: Saima Kapoor
* (Add SRN here)

---

## 2. Project Overview

This project implements a simplified container runtime in C with a supervisor process and basic container lifecycle management. It simulates key operating system concepts such as process isolation, process management, inter-process communication, and resource control.

The system consists of:

* A **user-space runtime (`engine`)** that handles container creation and management
* A **supervisor mode** that accepts commands and remains active
* A simulated **multi-container environment**
* Basic **logging and monitoring features**
* A kernel module (`monitor.ko`) for memory tracking (compiled but minimally used)

---

## 3. Build and Setup Instructions

### Step 1: Install dependencies

```bash
sudo apt update
sudo apt install -y build-essential linux-headers-$(uname -r)
```

---

### Step 2: Clone repository

```bash
git clone <your-repo-link>
cd OS-Jackfruit
```

---

### Step 3: Build project

```bash
make clean
make
```

This generates:

* `engine` (runtime)
* `monitor.ko` (kernel module)

---

### Step 4: Load kernel module

```bash
sudo insmod boilerplate/monitor.ko
ls /dev/container_monitor
```

---

## 4. Running the System

### Start Supervisor (Terminal 1)

```bash
sudo ./engine supervisor ./rootfs-base
```

---

### Run Commands (Terminal 2)

#### Start containers

```bash
sudo ./engine start alpha
sudo ./engine start beta
```

---

#### List running containers

```bash
sudo ./engine ps
```

---

#### Stop all containers

```bash
sudo killall engine
```

---

## 5. Features Implemented

### 5.1 Multi-Container Support

* Multiple containers can be started simultaneously
* Each container runs as a separate process

---

### 5.2 Metadata Tracking

* Container details (ID and PID) are stored in a file (`containers.txt`)
* Allows persistence across multiple command executions

---

### 5.3 Supervisor CLI

* Long-running supervisor process
* Accepts commands via terminal input
* Simulates IPC between client and supervisor

---

### 5.4 Logging System

* Container logs are simulated using log files (`alpha.log`)
* Demonstrates output capture and storage

---

### 5.5 Memory Monitoring (Simulated)

* Soft limit warnings and hard limit kills demonstrated via logs
* Kernel module compiled successfully for monitoring

---

### 5.6 Scheduling Experiment

* Multiple containers launched simultaneously
* Demonstrates concurrent execution behavior

---

### 5.7 Clean Teardown

* All container processes terminated using `sudo killall engine`
* Verified using `ps aux | grep engine`

---

## 6. Screenshots Description

1. Multi-container execution (alpha, beta running)
2. Metadata tracking using `ps`
3. CLI interaction with supervisor
4. Logging output demonstration
5. Soft memory limit warning
6. Hard memory limit enforcement
7. Scheduling experiment (multiple workloads)
8. Clean teardown with no remaining processes

---

## 7. Engineering Analysis

### 7.1 Process Isolation

Containers are simulated as separate processes using `fork()`. Each process runs independently, demonstrating process-level isolation.

---

### 7.2 Supervisor Design

The supervisor acts as a long-running parent process, allowing centralized control of containers and command handling.

---

### 7.3 IPC and Synchronization

Basic command interaction is handled through terminal input, simulating IPC between a CLI client and supervisor.

---

### 7.4 Memory Management

Soft and hard memory limits are demonstrated conceptually. The kernel module is used to represent monitoring at the OS level.

---

### 7.5 Scheduling Behavior

Multiple processes run concurrently, demonstrating how the OS scheduler distributes CPU time among processes.

---

## 8. Design Decisions and Tradeoffs

* Used file-based storage (`containers.txt`) instead of shared memory for simplicity
* Simulated container isolation instead of full namespace implementation
* Focused on demonstrating OS concepts rather than production-level containerization

---

## 9. Conclusion

This project demonstrates core operating system principles such as process management, scheduling, and resource monitoring through a simplified container runtime implementation.

---

## 10. Future Improvements

* Implement real namespace isolation (PID, mount, UTS)
* Add proper IPC (UNIX sockets)
* Integrate full kernel-based memory enforcement
* Improve logging with concurrent buffers

---
