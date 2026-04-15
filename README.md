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

This project demonstrates container lifecycle management and kernel-level monitoring.
The kernel module tracks memory usage and logs events.
User-space engine manages container processes using fork and exec.


## 5. Design Decisions and Tradeoffs

* Used fork() instead of full namespace isolation → simpler but less realistic
* Used printf logs instead of full IPC → easier implementation
* Kernel module kept minimal → easier debugging

---

## 6. Scheduler Experiment Results

CPU and memory workload programs demonstrate process behavior under load.
Memory hog shows increased memory usage.
CPU hog shows high CPU utilization.
