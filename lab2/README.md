# /proc filesystem

## Aim: Use /proc file system to gather basic information about your machine:

- Number of CPU cores
- Total memory and the fraction of free memory
- Number of processes currently running
- Number of processes in the running and blocked states
- Number of processes forked since the last bootup. How do you compare this value with the question above?
- The number of context switches performed since the last bootup for a particular process.

`/proc` is a virtual (pseudo) file system used to access process and kernel information at runtime.
    • /proc is not stored on disk.
    • It is created in memory by the kernel.
    • Provides a file-based interface to kernel data structures.

## Commands and Execution

(a) Number of CPU Cores
Command:
grep -c ^processor /proc/cpuinfo
Explanation:
    • /proc/cpuinfo lists details of each logical CPU.
    • Each CPU entry starts with the line processor : <number>.
    • grep -c counts how many lines match processor, i.e., how many cores/logical CPUs are available.
Sample Output:
4     -   Means the system has 4 CPU cores.

(b) Total Memory and Fraction of Free Memory
Command:
grep -E "MemTotal|MemFree" /proc/meminfo
Sample Output:
MemTotal:       8123456 kB
MemFree:        2345678 kB
To calculate the fraction of free memory:
awk '/MemTotal/ {total=$2} /MemFree/ {free=$2} END {print "Free Fraction = " free/total}' /proc/meminfo
awk is used for pattern scanning and text processing in Linux/Unix.It is especially powerful for extracting fields, filtering lines, and doing calculations on command output.
Sample Output:
Free Fraction = 0.2886

About 28.86% of memory is currently free.

(c) Number of Processes Currently Running
Command:
ps -e --no-headers | wc -l   
or using /proc directly:
ls -d /proc/[0-9]* | wc -l
Explanation:
    • Each active process has a directory /proc/<PID>.
    • Counting these directories gives the total number of active processes.
    • ps -e    → Displays all running processes in the system.
    • --no-headers  → Removes the header line (like PID TTY TIME CMD) so only process entries remain.
    • | (pipe) → Sends the output of ps to the next command.
    • wc -l → Counts the number of lines.
    • Top of Form
    • Bottom of Form
Sample Output:
203
