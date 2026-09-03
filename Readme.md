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

- **What is `execve`?**
  It is a Linux system call used to execute a program. Almost every process starts this way. By hooking `execve` with eBPF, you can monitor every process launch on a system.

- **Common eBPF Hooks**
  - **kprobe:** Attaches to any kernel function (dynamic).
  - **uprobe:** Attaches to user-space application functions.
  - **tracepoint:** Static hooks in the kernel source (stable API).
  - **XDP:** High-performance network packet processing at the driver level.
  - **LSM:** Security hooks for policy enforcement.

- **What is `attach_kprobe`?**
  It is a high-level helper function provided by tools like BCC or libbpf. It abstracts the complex system calls required to attach an eBPF program to a kernel function, making it easy to profile or trace system behavior.

- Lesson 1.2 eBPF Hello World
#!/usr/bin/python3

from bcc import BPF

 program = r"""
     int hello(void *ctx) {
       bpf_trace_printk("Hello World");
       return 0;
     }
"""

b = BPF(text=program)
syscall = b.get_Suscall_fname("execve")
b.attach_kprobe(event=syscall, fn_name="hello")



- Lesson 1.3 eBPF Maps
- Lesson 1.4 Introduction to bpftool

#### Additional Topics

- Learning
  - Understanding the eBPF Verifier
  - Kernel-User Space Communication
- Networking
  - Introduction to XDP (Express Data Path)
  - Traffic Control (TC) hooks

