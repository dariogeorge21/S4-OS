
### Operating Systems Lab -1

#### **Agent Task-1: Familiarisation of basic process commands**

This lab task focuses on understanding the basic commands used to inspect running processes, open files, and examine executable files in a Linux environment.

-----

1\. `ps` - Process Table

**Purpose:** Displays a snapshot of the currently running processes. By default, it shows processes associated with the current terminal session.

**Command:**

``` 
ps

```

**Output:**

``` 
    PID TTY          TIME CMD
   3409 pts/0    00:00:00 bash
   4634 pts/0    00:00:00 ps 
```

-----

2\. `ps -a` - All Processes with a Terminal

**Purpose:** Displays information about all processes, not just your own, that are associated with a terminal (`tty`).

**Command:**

``` 
ps -a

```

**Output:**

``` 
    PID TTY          TIME CMD
   1574 tty2     00:00:00 gnome-session-b
   4649 pts/0    00:00:00 ps

```

-----

3\. `ps -ef --forest` - Full Listing with Process Tree

**Purpose:** Displays a full-format listing (`-e`, `-f`) of every process on the system, and shows the process hierarchy using an ASCII art tree (`--forest`).

  - **UID:** User ID of the process owner.
  - **PID:** Process ID.
  - **PPID:** Parent Process ID.
  - **C:** CPU utilization.
  - **STIME:** Start time of the process.
  - **TTY:** Controlling terminal.
  - **TIME:** Cumulative CPU time.
  - **CMD:** Command being executed.

**Command:**

``` 
ps -ef --forest

```

**Output (Truncated for brevity and clarity):**

``` 
UID          PID    PPID  C STIME TTY          TIME CMD
root           2       0  0 09:58 ?        00:00:00 [kthreadd]
root           3       2  0 09:58 ?        00:00:00  \_ [pool_workqueue_release]
...
csea1       3409    3391  0 10:01 pts/0    00:00:00      \_ bash
csea1       4792    3409  0 10:20 pts/0    00:00:00          \_ ps -ef --forest
...

```

-----

4\. `ps -u csea1` - Processes by User

**Purpose:** Displays a user-oriented format list of all processes belonging to the specified user (`csea1`).

**Command:**

``` 
ps -u csea1

```

**Output (Truncated for brevity):**

``` 
    PID TTY          TIME CMD
   1460 ?        00:00:00 systemd
   1461 ?        00:00:00 (sd-pam)
   1467 ?        00:00:00 pipewire
... 
   3409 pts/0    00:00:00 bash
...

```

-----

5\. `top` - Monitor Processes Dynamically

**Purpose:** Provides a dynamic, real-time view of a running system. It shows a summary of system status and a list of processes currently being managed by the kernel. The output refreshes periodically.

**Command:**

``` 
top

```

**Output (Header explained):**

  - **PID:** Process ID
  - **USER:** User name of the process owner
  - **%CPU:** CPU usage since the last screen update
  - **%MEM:** Memory usage
  - **TIME+:** Total CPU time used by the task
  - **COMMAND:** Command name or path

**Output (Truncated for brevity):**

``` 
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   1661 csea1     20   0 4676584 285544 146408 S   7.0   3.6   1:46.10 gnome-shell
...
   3391 csea1     20   0  559096  56804  42832 S   1.7   0.7   0:15.04 gnome-terminal-
...

```

-----

6\. `fuser` - Identify Processes Using a File or Socket

**Purpose:** Lists the process IDs (PIDs) of processes that are currently using a specified file or file system. The `e` at the end of the PID indicates the process is using the file as an **executable** (e.g., actively running it).

**Command:**

``` 
fuser ./a.out

```

**Output:**

``` 
csea1@sjcet-GA-H81M-H:~$ fuser ./a.out 
/home/csea1/a.out:    5421e

```

-----

7\. `gdb` - GNU Debugger

**Purpose:** A powerful command-line debugger used to analyze and fix bugs in C, C++, and other compiled programs. The provided output shows a debugging session where a program called `main` (compiled from `div.c`) is run, hits an "Arithmetic exception" (SIGFPE) due to division by zero (`a=3`, `b=0`), and is then inspected.

**Key GDB Commands Used:**

  - `r` or `run`: Starts the program.
  - `print <variable>`: Displays the value of a variable.
  - `list`: Shows the source code around the current line.
  - `break main`: Sets a breakpoint at the `main` function.
  - `info locals`: Displays local variables in the current stack frame.
  - `quit`: Exits the GDB session.

**Command:**

``` 
gdb ./main

```

**Output (Key actions and results):**

``` 
...
Reading symbols from ./main...
(gdb) r
Starting program: /home/csea1/main 
...
Program received signal SIGFPE, Arithmetic exception.
0x0000555555555167 in main () at div.c:5
5        int c=a/b;
(gdb) print a
$1 = 3
(gdb) print b
$2 = 0
(gdb) list
1    #include<stdio.h>
2    
3    int main(){
4        int a=3,b=0;
5        int c=a/b;
6        printf("Result\n");
7        return 0;
8    }
(gdb) break main
Breakpoint 1 at 0x555555555155: file div.c, line 4.
...

```

-----

8\. `strings` - Print the Strings of Printable Characters in Files

**Purpose:** Searches for and prints any sequences of printable characters embedded in a binary or other file. This is useful for quickly inspecting the contents of an executable or object file for embedded text, library names, or version information.

**Command:**

``` 
strings str

```

**Output (Truncated):**

``` 
/lib64/ld-linux-x86-64.so.2
...
puts
libc.so.6
GLIBC_2.2.5
GLIBC_2.34
...
Divya Miss is my teacher!!
...
GCC: (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0
...

```

-----

9\. `objdump` - Display Information from Object Files

**Purpose:** Displays information about an object file, often used to disassemble the executable code. The `-d` option specifically shows the disassembly of the executable sections. This output shows the assembly code for the `.init`, `.plt`, `.plt.got`, `.plt.sec`, and `.text` sections, including the `main` function.

**Command:**

``` 
objdump -d a.out

```

**Output (Partial disassembly of `main`):**

``` 
...
0000000000001149 <main>:
    1149:    f3 0f 1e fa              endbr64 
    114d:    55                       push   %rbp
    114e:    48 89 e5                 mov    %rsp,%rbp
    1151:    48 8d 05 ac 0e 00 00     lea    0xeac(%rip),%rax        # 2004 <_IO_stdin_used+0x4>
    1158:    48 89 c7                 mov    %rax,%rdi
    115b:    e8 f0 fe ff ff           call   1050 <puts@plt>
    1160:    b8 00 00 00 00           mov    $0x0,%eax
    1165:    5d                       pop    %rbp
    1166:    c3                       ret    
...

```

-----

10\. `nm` - List Symbols from Object Files

**Purpose:** Lists the symbols (functions, global variables) contained within object files, executable files, or libraries.

  - **D:** Initialized data section.
  - **B:** Uninitialized data section (BSS).
  - **T:** Text (code) section.
  - **U:** Undefined symbol (needs to be resolved by a linker, e.g., standard library functions).

**Command:**

``` 
nm nm.o

```

**Output:**

``` 
0000000000000000 D a
0000000000000000 B b
0000000000000018 T main
                 U printf
0000000000000000 T sum

```

-----

11\. `file` - Determine File Type

**Purpose:** Examines the contents of a file to determine its type (e.g., executable, text, image, etc.).

**Command:**

``` 
file a.out

```

**Output:**

``` 
a.out: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=6843296ff9c32e3a2b86b70c9db850dfabbb072f, for GNU/Linux 3.2.0, not stripped

```

-----

12\. `od` - Octal Dump and Hex Dump Files

**Purpose:** Displays file contents in different human-readable formats (octal, hexadecimal, decimal, etc.).

**Command and Options:**

| Command               | Purpose/Format                                          | Output (Truncated)      |
| :-------------------- | :------------------------------------------------------ | :---------------------- |
| `od test.txt`         | Default octal format (`-o`).                            | `0000000 051517 000012` |
| `od -x test.txt`      | Hexadecimal format.                                     | `0000000 534f 000a`     |
| `od -c test.txt`      | ASCII/character format, escaped non-graphic characters. | `0000000   O   S  \n`   |
| `od -a test.txt`      | Named character format.                                 | `0000000   O   S  nl`   |
| `od -b test.txt`      | Octal byte format.                                      | `0000000 117 123 012`   |
| `od -t x1 test.txt`   | One-byte hexadecimal format.                            | `0000000 4f 53 0a`      |
| `od -c -N 5 test.txt` | Show ASCII characters, limit output to first 5 bytes.   | `0000000   O   S  \n`   |

-----

13\. `xxd` - Make a Hex Dump or Revert

**Purpose:** Creates a hexadecimal dump of a file (or standard input). It's commonly used to examine the binary content of a file.

**Command and Options:**

| Command          | Purpose/Format                                                | Output                                                       |
| :--------------- | :------------------------------------------------------------ | :----------------------------------------------------------- |
| `xxd s.txt`      | Standard hex dump (address, hex bytes, ASCII representation). | `00000000: 4f53 204c 6162 0a                        OS Lab.` |
| `xxd -p s.txt`   | Plain hex dump (just continuous hex characters).              | `4f53204c61620a`                                             |
| `xxd -c 8 s.txt` | Change the number of octets per line to 8.                    | `00000000: 4f53 204c 6162 0a    OS Lab.`                     |

