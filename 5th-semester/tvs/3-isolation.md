# Kernel and Processes Isolation

> Isolation is a very important concept in operating systems. It is the ability of the operating system to provide a secure environment for the execution of processes. In this chapter, we will discuss the isolation of processes and kernels.

* **CPU**: running time of each process is limited;
* **Mem**: processes can't access each other's memory;
* **I/O**: assure that the OS controls all I/O operations.

---

## Resources to Protect

* **Interrupt system**:
  * It needs to be protected, because its used to control the execution of processes, like the **scheduler** (to remove a process from the CPU and put another one in the CPU);
  * It is also used in the **I/O** system, to control the I/O devices;
* **Memory Access**:
  * Validate the memory access of a process, to assure that it is not accessing memory that it is not allowed to access;
  * **Validate all memory access operations**, like `fetch`, `load`, `store`, etc;
* **I/O Operations (if they exist)**: e.g. in x86, the instructions `in` and `out` are used to access I/O devices
* **Configuration mechanisms** of the previous resources.

---

## x86 Isolation Evolution

* **x86**: the first x86 processors didn't have any protection mechanism;
* 1978 - **i8086**: the beginning of the x86 architecture;
* 1982 - **i286**: the first x86 processor with protection mechanisms;
* 1985 - **i386**: the first x86 processor with **virtual memory** - [**IA-32**](https://en.wikipedia.org/wiki/IA-32);
* 2003 - **AMD64**: the first x86-64 processor - [**x86-64**](https://en.wikipedia.org/wiki/X86-64); extended the x86 architecture to 64 bits and removed unnecessary elements;

---

## Memory Protection

* Each process has its own **virtual memory** and a **memory map** (a table that maps the virtual memory to the physical memory);
* The CPU only receives the **virtual memory** address and uses the **memory map** to translate it to the **physical memory** address;
* The memory map address is stored in the register **CR3**, and the access to this register is a **privileged operation**;
* With the translation, there are configuration bits that define the **access permissions** of the memory:
  * read-only;
  * executable/non-executable;
  * **kernel space/user space**;

## Interrupt System Protection

* The system has a table with the configuration of the **interrupts**: the **Interrupt Descriptor Table** (**IDT**);
* This table address is stored in the register **IDTR**, inaccessible by the processes;
* Stored in **kernel space**;
* Contains the **interrupt handlers**, which are the functions that are executed when an interrupt occurs.

---

## Privilege Levels

* The x86 architecture has **privilege levels**:
  * **0**: the **most privileged level**, used by the **kernel**;
  * **1**: used by the kernel to execute processes;
  * **2**: used by the kernel to execute processes;
  * **3**: the **least privileged level**, used by the **processes**;
* x86 CPUs startup with the **privilege level 0** and transition to **level 3** when a process is executed;
* In each interruption, its indicated the **privilege level** that the interruption was executed and the **interruption handler**;
* To transition from a **higher privilege level** to a **lower privilege level**, the CPU needs to **switch the privilege level**, using the **interrupt system**.