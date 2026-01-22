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
1.  **Purpose:** Number of CPU Cores
    
      * **Command:**
        ``` bash
        grep -c ^processor /proc/cpuinfo
        
        ```
      * **Output:**
        ``` 
        4
        
        ```

2.  **Purpose:** Total Memory and Fraction of Free Memory
    
      * **Command 1 (Total and Free Memory):**
        ``` bash
        grep -E "MemTotal|MemFree" /proc/meminfo
        
        ```
      * **Output 1:**
        ``` 
        MemTotal:        3897820 kB
        MemFree:          475128 kB
        
        ```
      * **Command 2 (Calculate Fraction of Free Memory):**
        ``` bash
        awk '/MemTotal/ {total=$2} /MemFree/ {free=$2} END {print "Free Fraction = " free/total}' /proc/meminfo
        
        ```
      * **Output 2:**
        ``` 
        Free Fraction = 0.128011
        
        ```

3.  **Purpose:** Number of Processes Currently Running
    
      * **Command 1 (Using `ps` and `wc`):**
        ``` bash
        ps -e --no-headers | wc -l
        
        ```
      * **Command 2 (Using `/proc` directly):**
        ``` bash
        ls -d /proc/[0-9]* | wc -l
        
        ```
      * **Output (for both):**
        ``` 
        243
        
        ```

4.  **Purpose:** Number of Processes in Running and Blocked States
    
      * **Command (Running Processes):**
        ``` bash
        grep "procs_running" /proc/stat
        
        ```
      * **Output:**
        ``` 
        procs_running 1
        
        ```
      * **Command (Blocked Processes):**
        ``` bash
        grep "procs_blocked" /proc/stat
        
        ```
      * **Output:**
        ``` 
        procs_blocked 1
        
        ```

5.  **Purpose:** Number of Processes Forked Since Last Boot
    
      * **Command:**
        ``` bash
        grep "processes" /proc/stat
        
        ```
      * **Output:**
        ``` 
        processes 8366
        
        ```

6.  **Purpose:** Identify a Process ID (PID) for a specific process (bash)
    
      * **Command:**
        ``` bash
        ps -C bash
        
        ```
      * **Output:**
        ``` 
            PID TTY          TIME CMD
           6046 pts/0    00:00:00 bash
        
        ```
