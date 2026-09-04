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

Maps are key/value stores that allow eBPF programs to maintain state across events and share data with user-space applications.

### Example: Counting Executions per Process (PID)

```python
from bcc import BPF

# Define the eBPF program
prog = """
BPF_HASH(exec_counts, u32, u64); // Map: key=PID (u32), value=counter (u64)

int count_execve(void *ctx) {
    u32 pid = bpf_get_current_pid_tgid() >> 32;
    u64 *val, zero = 0;
    // Look up or initialize the counter for this PID
    val = exec_counts.lookup_or_try_init(&pid, &zero);
    if (val) (*val)++;
    return 0;
}
"""

b = BPF(text=prog)
b.attach_kprobe(event=BPF.get_syscall_fnname("execve"), fn_name="count_execve")

print("Collecting data... Press Ctrl+C to stop.")
# Access the map from Python:
# b["exec_counts"] would contain the data
```

#### Explanation of the Code:
1.  **`BPF_HASH(exec_counts, u32, u64);`**: Defines a hash map named `exec_counts`. The keys are 32-bit (Process IDs), and the values are 64-bit (execution counters).
2.  **`bpf_get_current_pid_tgid() >> 32`**: A helper function to extract the PID of the process triggering the `execve` event.
3.  **`exec_counts.lookup_or_try_init(&pid, &zero);`**: This is a BCC convenience function. It checks if there is already a counter for this PID in the map. If not, it creates a new entry initialized to `zero`.
4.  **`(*val)++`**: Once we have a pointer to the value, we increment it. Because this map is stored in kernel memory, these changes persist even after the `count_execve` function finishes execution.

- Lesson 1.4 Introduction to bpftool

 Is the command line utility fo inspecting manageing eBPF
 
 bpftool prog list
 ID is dynamically allocated and will always be unique
 Name is defined by source code
 Tag is a SHA sum of the program instructions

 bpftool map list
 bpftool map dump id 3 



### Lesson 2

### Lesson 2.1 XDP Programs

Dropping packets with eBPF, XDP_PASS, XDP_DROP, XDP_TX 


### Lesson 2.2 XDP Recap
XDP event is before kernetl network processs the packet.

can be applied layer 7

can be applid to layer 3 

TC= Traffic Control


#### Lesson 2.3 Introduction to Cillium

Cillium does not use kube-proxy

The cilium use CilliumNetworkPolicy to restrict traffic

#### Lesson 4

#### Lesson 4.1 eBPF for Security

eBPF is used for security in two distinct ways:

```
+-------------------------------------------------------------+
|                  eBPF Security Approaches                   |
+-----------------------------+-------------------------------+
|   Security Observability    |     Security Enforcement      |
|    (Detection/Tracing)      |     (Prevention/Blocking)     |
+-----------------------------+-------------------------------+
| Hook: kprobe, tracepoint    | Hook: LSM, XDP, seccomp       |
| Action: Capture & Log       | Action: Block/Deny/Modify     |
| Goal: "What happened?"      | Goal: "Should this happen?"   |
| Impact: Low (Monitoring)    | Impact: High (Blocking)       |
+-----------------------------+-------------------------------+
```

1. **Security Observability**: Gather events, compare them against a security policy, and report out-of-policy events.
2. **Security Enforcement**: Intercept actions at hooks (like LSM) and decide in real-time whether to allow or block them.

Example: packet-of-death, firewalls, Cillium network policy


#### Lesson 4.2 Attachement Points for Security

- **LSM (Linux Security Module) API**:
  eBPF programs can hook into the LSM framework, which is designed for security enforcement. By attaching eBPF programs to LSM hooks, you can perform fine-grained authorization checks (e.g., controlling file access, network socket creation, or process execution) at runtime, allowing you to deny actions before they are fully executed by the kernel.




#### Lesson 4.3 Cilium Tetragon

Cilium Tetragon is an eBPF-based security observability and runtime enforcement tool. Unlike traditional tools that rely on system call logging at the user space level, Tetragon runs entirely in the kernel using eBPF, allowing it to:

- **Enforce Policy in Real-time**: Block prohibited actions (e.g., executing a sensitive binary in a container) directly within the kernel.
- **Deep Visibility**: Observe process execution, file access, and network activity with very low overhead, as it operates at the kernel level without constant context switching.
- **Kubernetes Aware**: Understand Kubernetes context (namespaces, pod names), making it highly effective for securing modern cloud-native environments.



#### Lesson 5

#### Lesson 5.1: The bpf() system call

User space code uses bpf() syscall (mainly)
Verifier analyses the bytecode program
Bytecode isntructions are compiled into native machine instructions



#### Lesson 5.2 BPF Bytecode and the BPF Virtual Machine

Virutal machine implemented in sofware
Programs consists of eBPF bytecode instructions
10 genearl purpose regisers




#### Lesson 5.3 BPF Verification
eBPF Verfier

Analyses possible paths through an eBPF program
Checks that it is safe to run
 will run to completion
 won't crash



#### Additional Topics

- Learning
  - Understanding the eBPF Verifier
  - Kernel-User Space Communication
- Networking
  - Introduction to XDP (Express Data Path)
  - Traffic Control (TC) hooks


