# Operating Systems Lab

## Agent Task-1: Familiarisation of basic process commands

This lab task focuses on understanding the basic commands used to inspect running processes in a Linux environment.

-----

### 1\. `ps` - Process Table

**Purpose:** Displays a snapshot of the currently running processes. By default, it shows processes associated with the current terminal session.

**Command:**

``` bash
ps

```

**Output:**

``` 
    PID TTY          TIME CMD
   3409 pts/0    00:00:00 bash
   4634 pts/0    00:00:00 ps

```

-----

### 2\. `ps -a` - All Processes with a Terminal

**Purpose:** Displays information about all processes, not just your own, that are associated with a terminal (`tty`).

**Command:**

``` bash
ps -a

```

**Output:**

``` 
    PID TTY          TIME CMD
   1574 tty2     00:00:00 gnome-session-b
   4649 pts/0    00:00:00 ps

```

-----

### 3\. `ps -ef --forest` - Full Listing with Process Tree

**Purpose:** Displays a full-format listing (`-e`, `-f`) of every process on the system, and shows the process hierarchy using an ASCII art tree (`--forest`).

  * **UID:** User ID of the process owner.
  * **PID:** Process ID.
  * **PPID:** Parent Process ID.
  * **C:** CPU utilization.
  * **STIME:** Start time of the process.
  * **TTY:** Controlling terminal.
  * **TIME:** Cumulative CPU time.
  * **CMD:** Command being executed.

**Command:**

``` bash
ps -ef --forest

```

**Output (Truncated for brevity and clarity):**

``` 
UID          PID    PPID  C STIME TTY          TIME CMD
root           2       0  0 09:58 ?        00:00:00 [kthreadd]
root           3       2  0 09:58 ?        00:00:00  \_ [pool_workqueue_release]
root           1       0  0 09:58 ?        00:00:01 /sbin/init splash
... (various system processes)
csea1       1571    1456  0 09:59 tty2     00:00:00      \_ /usr/libexec/gdm-wayland-session env GNOME_SHELL_SESSION_MODE=ubuntu /usr/bin/gnome-session --session=ubuntu
csea1       1574    1571  0 09:59 tty2     00:00:00          \_ /usr/libexec/gnome-session-binary --session=ubuntu
...
csea1       3409    3391  0 10:01 pts/0    00:00:00      \_ bash
csea1       4792    3409  0 10:20 pts/0    00:00:00          \_ ps -ef --forest
...

```

-----

### 4\. `ps -u csea1` - Processes by User

**Purpose:** Displays a user-oriented format list of all processes belonging to the specified user (`csea1`).

**Command:**

``` bash
ps -u csea1

```

**Output (Truncated for brevity):**

``` 
    PID TTY          TIME CMD
   1460 ?        00:00:00 systemd
   1461 ?        00:00:00 (sd-pam)
   1467 ?        00:00:00 pipewire
   1468 ?        00:00:00 pipewire-media-
   1469 ?        00:00:00 pulseaudio
... (all processes running under user 'csea1')
   3409 pts/0    00:00:00 bash
...

```

-----

### 5\. `top` - Monitor Processes Dynamically

**Purpose:** Provides a dynamic, real-time view of a running system. It shows a summary of system status and a list of processes currently being managed by the kernel. The output refreshes periodically.

**Command:**

``` bash
top

```

**Output (Header explained):**

  * **PID:** Process ID
  * **USER:** User name of the process owner
  * **PR:** Priority
  * **NI:** Nice value (user-space priority)
  * **VIRT:** Virtual memory usage (KiB)
  * **RES:** Resident memory size (KiB) - physical memory currently used
  * **SHR:** Shared memory size (KiB)
  * **S:** Process status (e.g., S=sleeping, R=running)
  * **%CPU:** CPU usage since the last screen update
  * **%MEM:** Memory usage
  * **TIME+:** Total CPU time used by the task
  * **COMMAND:** Command name or path

**Output (Truncated for brevity):**

``` 
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   1661 csea1     20   0 4676584 285544 146408 S   7.0   3.6   1:46.10 gnome-shell
    623 avahi     20   0   11320   7424   3584 S   1.7   0.1   0:28.60 avahi-daemon
   3391 csea1     20   0  559096  56804  42832 S   1.7   0.7   0:15.04 gnome-terminal-
    889 mysql     20   0 2308448 396112  35840 S   0.7   4.9   0:11.90 mysqld
... (various other processes and system info)

```
