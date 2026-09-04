# eBPF Course Final Quiz

Test your knowledge on eBPF concepts.

---

### 1. Which of the following statements about eBPF bytecode are true? (Select all that apply)

A. Compilers such as gcc, clang, and the Rust compiler can convert source code to eBPF bytecode.
B. An eBPF program consists of a series of eBPF bytecode instructions.
C. eBPF bytecode is executed in a software virtual machine in the kernel.
D. eBPF bytecode is the same as machine code.

---

### 2. What causes an eBPF program to run?

A. An eBPF program is attached to an event, and runs whenever that event occurs.
B. An eBPF program runs whenever someone executes a program on the same machine.
C. An eBPF program is contained in a network packet, and it runs when it reaches the kernel’s networking stack.
D. An eBPF program runs when a user runs the command `bpftool prog show`.

---

### 3. Which of the following commands lists the eBPF programs currently loaded in the kernel?

A. `bpftool prog show`
B. `bpf prog list`
C. `bpftool prog list`
D. `bpftool list`

---

### 4. What is the correct order for these events to occur? (X, Y, Z)
X. The eBPF verifier analyzes the eBPF bytecode.
Y. Userspace code tells the kernel which event to attach the program to.
Z. Userspace code loads an eBPF program into the kernel with the bpf() syscall.

A. Y -> X -> Z
B. X -> Z -> Y
C. Z -> X -> Y
D. Z -> Y -> X

---

### 5. What is the system call that creates an eBPF map?

A. `bpf()` with `BPF_MAP_CREATE`
B. `bpf()` with `BPF_MAP_LOAD`
C. `ioctl()` with `BPF_MAP_CREATE`
D. `ioctl()` with `BPF_MAP_LOAD`

---

### 6. What is the main advantage of using eBPF observability tools built specifically for Kubernetes environments (e.g., Inspektor Gadget, Hubble)?

A. These tools are easier to use because they have a graphical UI.
B. Other tools like BCC or bpftrace can’t detect events triggered by applications in Kubernetes pods.
C. Other tools like BCC or bpftrace can’t run in a Kubernetes environment.
D. These tools show information about Kubernetes identities, such as the name of pods and nodes involved in an event.

---

### 7. What are some reasons why eBPF is great for observability? (Select all that apply)

A. Applications don’t need to be modified or reconfigured.
B. eBPF tools can be run by any user regardless of privilege.
C. Asynchronous data sharing via eBPF maps is efficient.
D. Applications don't need to be restarted.

---

### 8. Which of these are examples of security enforcement? (Select all that apply)

A. Preventing an application from reading from a forbidden file.
B. Dropping packets that violate network security policy.
C. Alerting the user about an unauthorized file read attempt.
D. Generating logs for forbidden network connections.

---

### 9. What actions can an eBPF program attached to XDP take? (Select all that apply)

A. Report information about the user space process that sent the packet.
B. Inspect and report information about the packet via an eBPF map.
C. Modify the packet and pass it to the network stack (`XDP_PASS`).
D. Drop the packet (`XDP_DROP`).

---

### 10. Which of these are capabilities of Cilium? (Select all that apply)

A. Connecting Kubernetes apps to external services via Egress Gateway.
B. Connecting services across multiple Kubernetes clusters.
C. Running containerized applications as a container runtime.
D. Encrypting traffic between Kubernetes services.

---

### 11. Which actions can you perform with `bpftool`? (Select all that apply)

A. List loaded eBPF programs.
B. List available eBPF helper functions.
C. Edit an eBPF program loaded in the kernel.
D. View debug tracing emitted by programs.

---

### 12. Which statements about Tetragon are true? (Select all that apply)

A. Tetragon provides a service map for Kubernetes.
B. Tetragon filters events against policies within the kernel.
C. Tetragon can issue a synchronous SIGKILL for out-of-policy events.
D. Tetragon generates rich metadata for events (PID, pod, etc.).

---

### 13. What project is Tetragon part of?

A. Iovisor
B. BCC
C. Inspektor Gadget
D. Cilium

---

### 14. Which of these are BCC tools? (Select all that apply)

A. `opensnoop`
B. `execsnoop`
C. `gadget`
D. `hubble`
