# Lab 2: /proc Filesystem

## Introduction

The `/proc` filesystem is a **virtual filesystem** in Linux that provides access to process and kernel information at runtime. 

**Key Points:**
- `/proc` is not stored on disk - it exists only in memory
- Created dynamically by the kernel
- Provides a file-based interface to view system and process information
- Uses zero disk space

---

## Aim

To use the `/proc` filesystem to gather basic information about the system: 
- Number of CPU cores
- Total memory and fraction of free memory
- Number of processes currently running
- Number of processes in running and blocked states
- Number of processes forked since last bootup
- Number of context switches for a specific process

---

## Commands and Outputs

### Command 1: Number of CPU Cores

**Purpose:** Find the total number of CPU cores in the system

**Command:**
```bash
grep -c ^processor /proc/cpuinfo
```

**Output:**
```
4
```

**Explanation:** The system has 4 CPU cores. 

---

### Command 2: Total Memory

**Purpose:** Display total and free memory

**Command:**
```bash
grep -E "MemTotal|MemFree" /proc/meminfo
```

**Output:**
```
MemTotal:        3897820 kB
MemFree:          475128 kB
```

**Explanation:** 
- Total Memory: 3,897,820 kB (~3.8 GB)
- Free Memory: 475,128 kB (~464 MB)

---

### Command 3: Fraction of Free Memory

**Purpose:** Calculate what fraction of memory is free

**Command:**
```bash
awk '/MemTotal/ {total=$2} /MemFree/ {free=$2} END {print "Free Fraction = " free/total}' /proc/meminfo
```

**Output:**
```
Free Fraction = 0.128011
```

**Explanation:** Approximately 12.8% of memory is free, meaning 87.2% is in use.

---

### Command 4: Number of Processes Currently Running

**Purpose:** Count all active processes in the system

**Command (Method 1):**
```bash
ps -e --no-headers | wc -l
```

**Command (Method 2):**
```bash
ls -d /proc/[0-9]* | wc -l
```

**Output:**
```
243
```

**Explanation:** There are 243 processes currently active in the system.

---

### Command 5: Processes in Running State

**Purpose:** Find the number of processes currently running on CPU

**Command:**
```bash
grep "procs_running" /proc/stat
```

**Output:**
```
procs_running 1
```

**Explanation:** Only 1 process is actively running on CPU at this moment.

---

### Command 6: Processes in Blocked State

**Purpose:** Find the number of processes blocked (waiting for I/O)

**Command:**
```bash
grep "procs_blocked" /proc/stat
```

**Output:**
```
procs_blocked 1
```

**Explanation:** 1 process is currently blocked, waiting for I/O operations to complete.

---

### Command 7: Total Processes Forked Since Boot

**Purpose:** Find total number of processes created since system startup

**Command:**
```bash
grep "processes" /proc/stat
```

**Output:**
```
processes 8366
```

**Explanation:** 
- 8,366 processes have been created since boot
- This is much higher than current processes (243) because many processes are short-lived
- This is a **cumulative count** of all processes ever created since boot

---

### Command 8: Find Process ID (PID)

**Purpose:** Get the PID of a specific process (bash shell in this example)

**Command:**
```bash
ps -C bash
```

**Output:**
```
    PID TTY          TIME CMD
   6046 pts/0    00:00:00 bash
```

**Explanation:** The bash process is running with PID 6046.

---

### Command 9: Context Switches for a Process

**Purpose:** Find the number of context switches for a specific process

**Command:**
```bash
grep ctxt_switches /proc/6046/status
```

**Output Example:**
```
voluntary_ctxt_switches:        150
nonvoluntary_ctxt_switches:     25
```

**Explanation:**
- **Voluntary:** Process gave up CPU willingly (waiting for I/O, sleeping, etc.)
- **Nonvoluntary:** Process was forced to give up CPU (time expired, preempted by higher priority)

**Note:** Replace `6046` with your actual process PID from Command 8.

---

## Summary of Results

| Command | Purpose | Result |
|---------|---------|--------|
| 1 | CPU Cores | 4 |
| 2 | Total Memory | 3,897,820 kB |
| 2 | Free Memory | 475,128 kB |
| 3 | Free Memory Fraction | 0.128 (12.8%) |
| 4 | Current Processes | 243 |
| 5 | Running Processes | 1 |
| 6 | Blocked Processes | 1 |
| 7 | Processes Since Boot | 8,366 |
| 8 | Example Process PID | 6046 (bash) |
| 9 | Context Switches | Varies by process |

---

## Conclusion

- The `/proc` filesystem provides easy access to system and process information
- We can monitor CPU, memory, and process statistics using simple commands
- The difference between current processes (243) and total forked processes (8,366) shows that most processes are short-lived
- Most processes are in waiting/sleeping states, not actively running

---

**Name:** [Dario George]
**Roll No:** [32]  
**Date:** [22/01/2026]
