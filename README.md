##team
Name: Gowri TN
SRN: PES1UG24AM421

Name: Anjali Arun
SRN: PES1UG24AM421

-------
# task1- multi-container runtime
##project overview

This project implements a lightweight container runtime in C.
In Task 1, we build a **multi-container system** managed by a long-running supervisor process.
Each container is isolated using Linux namespaces and has its own filesystem and process view.

---

##Build Instructions

```bash
cd boilerplate
make
```
---

## Root Filesystem Setup
```bash
cd ..
mkdir rootfs-base
wget https://dl-cdn.alpinelinux.org/alpine/v3.20/releases/aarch64/alpine-minirootfs-3.20.3-aarch64.tar.gz
tar -xzf alpine-minirootfs-3.20.3-aarch64.tar.gz -C rootfs-base
cp -a rootfs-base rootfs-alpha
cp -a rootfs-base rootfs-beta
```
---

## Running the Supervisor
```bash
cd boilerplate
sudo ./engine supervisor ../rootfs-base
```
Expected output:
```
Supervisor started with rootfs: ../rootfs-base
Container alpha started with PID XXXX
Container beta started with PID YYYY
```
---
##  Screenshots
### 1. Host view of Multi-container supervision 
Shows two containers running under one supervisor with distinct host PIDs.

![Host processes](/home/ubuntu/OS-Jackfruit/screenshots/task1/host_processes.png)

---

### 2. Container isolation (Inside container)

Shows:
* `hostname` → container-specific identity
* `ps` → only container processes
* `/proc` → mounted inside container

![Container isolation](/home/ubuntu/OS-Jackfruit/screenshots/task1/isolated_container.png)

---

## note

* Multiple containers run concurrently under a single supervisor
* Each container has isolated:
  * Process space (PID namespace): isolates process IDs
  * Hostname (UTS namespace): allows separate hostnames
  * Filesystem (chroot + mount namespace)
* `/proc` reflects container processes, not host processes
* Containers cannot access host filesystem
* **Mount namespace** → separate filesystem mounts
---
* Chroot:
* Changes the root directory of the container
* Restricts filesystem visibility
---
* Proc filesystem:
* Kernel-provided virtual filesystem
* Required for tools like `ps`
* Must be mounted inside container

---
Root filesystem directories (`rootfs-*`) are not committed to Git
* Containers must use separate writable rootfs copies
* Architecture of rootfs must match host (ARM vs x86)

##  Verification

Inside container:

```bash
hostname
ps
ls /proc
```

Host system:

```bash
ps aux | grep sh
```

---

