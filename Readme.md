#### Course: Getting Started with eBPF 

- Lesson 1.1 What is eBPF

Initially was to filter packtet, but now it can be used to intercept some events like
kprobes, uprobes, tracpeoints,network psackrts ,linus security module,perf events

```
      User Space          Kernel Space
    +-------------+     +-------------------+
    |  eBPF Prog  | --> |     eBPF VM       |
    |  (loader)   |     | (Verifier/JIT)    |
    +-------------+     +-------------------+
             |                   ^
             v                   |
    +-------------------+        |
    |   eBPF Map        | <------+
    +-------------------+
```

eBPF Verifier= ensures programs can't crash or hang the kernel.



### Example: kprobe for `sys_execve`

```c
#include <linux/bpf.h>
#include <bpf/bpf_helpers.h>

// This attaches the program to the kprobe for sys_execve
SEC("kprobe/sys_execve")
int BPF_KPROBE(trace_execve) {
    // This will print to the kernel trace pipe
    bpf_printk("An execve syscall was just triggered!\n");
    return 0;
}

char LICENSE[] SEC("license") = "GPL";
```

### Understanding Key eBPF Concepts

- **What is `execve` and what else exists?**
  `execve` is a system call to execute a program. It is the most common way processes start. Other important system calls include:
  - Process creation: `fork`, `clone`.
  - File I/O: `open`, `read`, `write`, `close`.
  - Networking: `socket`, `bind`, `connect`, `accept`.
  Hooking these allows you to track everything from file access to network connections.

- **Common eBPF Hooks**
  - **kprobe:** Attaches to any kernel function (dynamic).
  - **uprobe:** Attaches to user-space application functions.
  - **tracepoint:** Static hooks in the kernel source (stable API).
  - **XDP:** High-performance network packet processing at the driver level.
  - **LSM:** Security hooks for policy enforcement.

- **What is `attach_kprobe`?**
  It is a high-level helper function provided by tools like BCC or libbpf. It abstracts the complex system calls required to attach an eBPF program to a kernel function, making it easy to profile or trace system behavior.

- Lesson 1.2 eBPF Hello World
```python
#!/usr/bin/python3
from bcc import BPF

# Define the eBPF program
program = r"""
    int hello(void *ctx) {
      bpf_trace_printk("Hello World\\n");
      return 0;
    }
"""

# Load the program
b = BPF(text=program)

# Get the correct kernel function name for the syscall
syscall = BPF.get_syscall_fnname("execve")

# Attach the kprobe
b.attach_kprobe(event=syscall, fn_name="hello")

# Read trace output
b.trace_print()
```

Once you run this script, it will sit and wait for `execve` calls. If you open another terminal and run commands like `ls` or `whoami`, you will see the "Hello World" output appear in the first terminal, confirming that these commands triggered the `execve` system call, which was then intercepted by your eBPF program.



- Lesson 1.3 eBPF Maps




- Lesson 1.4 Introduction to bpftool

#### Additional Topics

- Learning
  - Understanding the eBPF Verifier
  - Kernel-User Space Communication
- Networking
  - Introduction to XDP (Express Data Path)
  - Traffic Control (TC) hooks

