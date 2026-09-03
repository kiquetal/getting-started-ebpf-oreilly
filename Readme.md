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

- Lesson 1.2 eBPF Hello World





- Lesson 1.3 eBPF Maps
- Lesson 1.4 Introduction to bpftool

#### Additional Topics

- Learning
  - Understanding the eBPF Verifier
  - Kernel-User Space Communication
- Networking
  - Introduction to XDP (Express Data Path)
  - Traffic Control (TC) hooks

