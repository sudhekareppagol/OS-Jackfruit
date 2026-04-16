# OS Jackfruit Project - Container Runtime & Memory Monitor

## 1. Team Information

* Name: sudhe 
* SRN: PES1UG24CS695
* Name:tanisha
* SRN:PES1UG24CS635

---
# Build the project
make

# Load kernel module
sudo insmod monitor.ko

# Verify device
ls /dev/container_monitor

# Start supervisor
sudo ./engine supervisor ./rootfs-base

# Create container root filesystems
cp -a rootfs-base rootfs-alpha
cp -a rootfs-base rootfs-beta

# Start containers (in another terminal)
sudo ./engine start alpha ./rootfs-alpha /bin/sh
sudo ./engine start beta ./rootfs-beta /bin/sh

# List containers
sudo ./engine ps

# View logs
sudo ./engine logs alpha

# Stop containers
sudo ./engine stop alpha
sudo ./engine stop beta

# Check kernel logs
dmesg | tail

# Unload module
sudo rmmod monitor
---


## 3. Demo with Screenshots

1. Container start output
2. ps command output
3. logs command output
4. Kernel module loaded (dmesg)

---

## 4. Engineering Analysis
1. Isolation Mechanisms

Containers use process isolation to run independently. In this project, isolation is partially achieved using separate root filesystems (rootfs-alpha, rootfs-beta) and separate processes. Each container runs its own /bin/sh, giving it an independent execution environment. However, since full namespace isolation is not implemented, containers still share the host kernel.

2. Supervisor and Process Lifecycle

A long-running supervisor process is used to manage containers. It starts containers using fork() and exec(), tracks their state, and ensures proper cleanup. This avoids zombie processes and allows multiple containers to run concurrently.

3. IPC, Threads, and Synchronization

The project uses basic IPC through command-line interaction. Logs are handled through a simple bounded-buffer style approach. Since full IPC (like sockets) is not implemented, communication is limited but sufficient for demonstration.

4. Memory Management and Enforcement

The kernel module monitors memory usage and enforces soft and hard limits.

Soft limit → warning is generated
Hard limit → container is terminated
This shows how the kernel controls memory usage for processes.

5. Scheduling Behavior

Different workloads (cpu_hog and io_pulse) demonstrate Linux scheduling. CPU-bound processes consume more CPU time, while I/O-bound processes wait frequently, showing how the scheduler balances tasks.

## 5. Design Decisions and Tradeoffs
Namespace Isolation
Choice: Did not implement full namespaces
Tradeoff: Easier but less isolation
Reason: Simpler implementation for learning

Supervisor Design
Choice: Single supervisor process
Tradeoff: Central control but single point of failure
Reason: Easier container management

IPC / Logging
Choice: Simple printf/log system
Tradeoff: Limited communication
Reason: Easier debugging

Kernel Monitor
Choice: Minimal kernel module
Tradeoff: Fewer features
Reason: Stability and simplicity

Scheduling Experiment
Choice: Used cpu_hog and io_pulse
Tradeoff: Basic experiment
Reason: Clearly shows scheduling differences

---

## 6. Scheduler Experiment Results
Experiment Setup

Two workloads were used:

cpu_hog → CPU-intensive
io_pulse → I/O-intensive

Observations
CPU-bound process used high CPU continuously
I/O-bound process showed lower CPU usage
Scheduler gave fair time to both processes

Conclusion

Linux scheduler prioritizes fairness and responsiveness. CPU-heavy tasks get more CPU, while I/O tasks wait, demonstrating efficient scheduling.
