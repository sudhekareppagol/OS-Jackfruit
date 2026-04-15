# OS Jackfruit Project - Container Runtime & Memory Monitor

## 1. Team Information

* Name: [Your Name]
* SRN: [Your SRN]

---

## 2. Build, Load, and Run Instructions

### Build the Project

```
make clean
make
```

### Load Kernel Module

```
sudo insmod monitor.ko
```

### Verify Device

```
ls /dev/container_monitor
```

### Start Container

```
sudo ./engine start alpha ./rootfs-base /bin/sh
```

### List Containers

```
sudo ./engine ps
```

### View Logs

```
sudo ./engine logs alpha
```

### Stop Module

```
sudo rmmod monitor
```

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

---

## 5. Design Decisions and Tradeoffs

* Used fork() instead of full namespace isolation → simpler but less realistic
* Used printf logs instead of full IPC → easier implementation
* Kernel module kept minimal → easier debugging

---

## 6. Scheduler Experiment Results

CPU and memory workload programs demonstrate process behavior under load.
Memory hog shows increased memory usage.
CPU hog shows high CPU utilization.
