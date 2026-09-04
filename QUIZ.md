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

### 5. Which of the following are reasons why the eBPF Verifier might reject a program? (Select all that apply)

A. The program makes a call to an invalid helper function for its context.
B. The program has a loop that exceeds the eBPF complexity limit.
C. The program dereferences a pointer without checking if it is null.
D. The program calls a GPL-licensed helper function, but it does not declare a GPL-compatible license.

(Correct Answers: A, B, C, D)

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

### 9. Identify which of the following scenarios are use cases for dropping network packets using an eBPF program. (Select all that apply)

A. Mitigating a kernel vulnerability in the network stack.
B. Implementing a firewall.
C. Enforcing network policies.
D. Mitigating a kernel vulnerability in file access.

(Correct Answers: A, B, C)

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

(Correct Answers: A, B, D)

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

---

## Verified Answer Key

Based on the O'Reilly *Learning eBPF* course/book by Liz Rice, cross-checked against ebpf.io, cilium.io, and tetragon.io documentation.

| # | Correct Answer(s) | Notes |
|---|-------------------|-------|
| 1 | A, B, C | D is wrong: bytecode is JIT-compiled to machine code, it is not itself machine code. |
| 2 | A | eBPF programs are attached to events and run when the event fires. |
| 3 | A (`bpftool prog show`) | `bpftool prog list` is an accepted alias, but the course uses `show`. |
| 4 | C (Z → X → Y) | Load with `bpf()` syscall, verifier checks during load, then attach to event. |
| 5 | A, B, C, D | **Corrected** — D is a real rejection cause: a GPL-only helper called without a GPL-compatible license declaration is rejected. |
| 6 | D | Kubernetes-aware tools enrich events with pod/node identity. |
| 7 | A, C, D | B is wrong: eBPF requires privilege (root / CAP_BPF), not any user. |
| 8 | A, B | Enforcement = prevent/drop. C (alert) and D (log) are observability, not enforcement. |
| 9 | A, B, C | D (file access) is not a network-packet-dropping use case. |
| 10 | A, B, D | C is wrong: Cilium is not a container runtime. |
| 11 | A, B, D | C is wrong: you cannot edit a program already loaded in the kernel. |
| 12 | B, C, D | A is wrong: a service map is a Hubble feature, not Tetragon. |
| 13 | D | Tetragon is part of the Cilium project. |
| 14 | A, B | `opensnoop` and `execsnoop` are BCC tools; `gadget` is Inspektor Gadget and `hubble` is Cilium. |

