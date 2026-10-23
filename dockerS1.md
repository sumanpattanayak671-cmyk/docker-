# Docker Session 1: Physical Servers, Virtualization, and Containers

> **Learning goal:** Understand physical servers, why virtualization is
> useful, how virtual machines work, what containers are, and the key
> differences between VMs and containers.

## Table of Contents

1.  [Introduction](#1-introduction)
2.  [Architecture of Physical
    Servers](#2-architecture-of-physical-servers)
3.  [Main Components of a Physical
    Server](#3-main-components-of-a-physical-server)
4.  [How a Physical Server Works](#4-how-a-physical-server-works)
5.  [Physical Server vs. Virtual
    Server](#5-physical-server-vs-virtual-server)
6.  [Problems Associated with Physical
    Servers](#6-problems-associated-with-physical-servers)
7.  [Hardware Virtualization](#7-hardware-virtualization)
8.  [Hypervisors: Type 1 and Type 2](#8-hypervisors-type-1-and-type-2)
9.  [Problems with Hardware
    Virtualization](#9-problems-with-hardware-virtualization)
10. [OS Virtualization:
    Containerization](#10-os-virtualization-containerization)
11. [Advantages and Use Cases of
    Containers](#11-advantages-and-use-cases-of-containers)
12. [Hardware Virtualization vs. OS
    Virtualization](#12-hardware-virtualization-vs-os-virtualization)
13. [How Docker Helps](#13-how-docker-helps)
14. [Quick Revision](#14-quick-revision)
15. [Practice Questions](#15-practice-questions)

------------------------------------------------------------------------

## 1. Introduction

Computers and servers provide the resources needed to run applications,
websites, databases, and other services.

There are three important ideas in this session:

-   **Physical server:** A real computer made of physical components
    such as a CPU, RAM, storage, and network interfaces.
-   **Virtual machine (VM):** A software-based computer that runs its
    own operating system on virtualized hardware.
-   **Container:** An isolated environment for an application that
    shares the host operating system's kernel.

### A simple way to remember

-   A **physical server** is the actual machine.
-   A **VM** behaves like a separate computer.
-   A **container** packages and isolates an application and its
    required files while sharing the host kernel.

## 2. Architecture of Physical Servers

### 2.1 What is physical server architecture?

Physical server architecture is the design and arrangement of the
physical hardware components inside a server. These components work
together to process data, store files, communicate over a network, and
provide services to users.

In simple words, a **physical server is a powerful computer designed to
provide services to other computers over a network**.

Physical servers are commonly used in data centers and enterprise
environments to host applications, databases, websites, and other
services.

### 2.2 Real-world example

When you open a website or watch a video online:

1.  Your computer or phone sends a request.
2.  The request travels through a network to a server.
3.  The server processes the request and retrieves the required data.
4.  The server sends a response back to your device.

Physical servers in data centers provide the computing, storage, and
networking resources needed to perform these tasks.

## 3. Main Components of a Physical Server

### 3.1 CPU (Central Processing Unit)

**Purpose:** Executes instructions, performs calculations, and processes
requests.

Think of the CPU as the server's brain. It carries out the instructions
needed by the operating system and applications.

**Example:** When a web server receives a request, the CPU processes the
instructions required to handle that request.

### 3.2 RAM (Random Access Memory)

**Purpose:** Temporarily stores data and programs that the CPU is
currently using.

RAM helps a server handle active applications and workloads. More
available RAM can allow a server to support more work, depending on the
workload.

**Important:** RAM is temporary working memory; it is not the same as
permanent storage.

### 3.3 Storage (HDD/SSD)

**Purpose:** Stores the operating system, applications, databases,
files, and other data persistently.

-   **HDD:** A hard disk drive that stores data on rotating disks.
-   **SSD:** A solid-state drive that uses flash memory and has no
    moving disk parts.

**Example:** A server's SSD may store RHEL, Apache HTTP Server, website
files, and application data.

### 3.4 Network Interface Card (NIC)

**Purpose:** Connects the server to a network through Ethernet or
another supported network technology.

The NIC enables the server to send and receive network traffic.

**Example:** A server uses its NIC to communicate with client computers
and other servers.

### 3.5 Motherboard

**Purpose:** Connects and allows communication between the CPU, RAM,
storage, network cards, and other hardware components.

It provides the physical platform into which many of the server's
components are installed.

### 3.6 Power Supply Unit (PSU)

**Purpose:** Supplies electrical power to the server.

Enterprise servers may use redundant power supplies. Redundancy can help
maintain operation if one power supply fails, provided the system is
designed and configured for it.

### 3.7 Cooling System

**Purpose:** Removes heat generated by the server's components.

Fans and heat sinks help keep components within safe operating
temperatures. In data centers, cooling infrastructure is also important.

### 3.8 Component summary

  Component        Main responsibility
  ---------------- -----------------------------------------------------
  CPU              Executes instructions and processes workloads
  RAM              Holds data and programs currently in use
  HDD/SSD          Stores the OS, applications, and files persistently
  NIC              Connects the server to a network
  Motherboard      Connects hardware components
  PSU              Supplies electrical power
  Cooling system   Helps prevent overheating

## 4. How a Physical Server Works

A typical request-and-response process works like this:

1.  **Client computer:** A user requests a website or service.
2.  **Network and switch:** The request travels across the network to
    the server.
3.  **NIC:** The server's network interface receives the network
    traffic.
4.  **RAM:** Active application data and working information can be held
    in memory.
5.  **CPU:** The processor executes the instructions needed to handle
    the request.
6.  **Storage:** The server reads files or data from storage when
    required.
7.  **Response:** The server sends the result back through its NIC and
    the network to the client.

The exact order of CPU, RAM, and storage activity depends on the
application and request.

### Example: A RHEL web server

Imagine a physical server running Red Hat Enterprise Linux (RHEL):

-   **CPU:** Processes application instructions and user requests.
-   **RAM:** Holds active application data.
-   **SSD:** Stores RHEL, Apache HTTP Server, website files, and other
    data.
-   **NIC:** Connects the server to the network.
-   **Apache HTTP Server:** Serves web pages to users.

One physical machine can also run multiple VMs using a hypervisor such
as KVM.

## 5. Physical Server vs. Virtual Server

  -----------------------------------------------------------------------
  Physical server                     Virtual server
  ----------------------------------- -----------------------------------
  An actual hardware machine          A software-based machine

  Has physical CPU, RAM, and storage  Uses virtualized CPU, RAM, storage,
  components                          and other devices

  Runs directly on hardware or may    Runs on a physical host through a
  host a hypervisor                   virtualization platform

  Example: Dell PowerEdge server      Example: RHEL VM running on KVM
  -----------------------------------------------------------------------

**Remember:** Physical server architecture describes the hardware
foundation on which operating systems, applications, databases, and VMs
run.

## 6. Problems Associated with Physical Servers

Physical servers are powerful, but using dedicated hardware can create
cost, maintenance, and capacity challenges.

### 6.1 High initial cost

Purchasing servers requires investment in CPUs, RAM, storage, networking
equipment, racks, and related infrastructure.

**Example:** A company must buy a physical server before it can host its
own applications on that hardware.

### 6.2 High maintenance

Servers require hardware maintenance, software updates, cleaning,
monitoring, and sometimes component replacement.

**Example:** If a server's power supply fails, a technician may need to
replace it.

### 6.3 Hardware failure

CPUs, RAM modules, storage drives, and power supplies can fail. A
failure may interrupt services or cause data loss if appropriate
protections are not in place.

**Example:** If a storage drive fails and no usable backup exists,
important data may be lost.

### 6.4 Limited scalability

Increasing capacity may require adding components or purchasing
additional hardware.

**Example:** If a server needs more RAM, compatible memory modules may
need to be purchased and installed.

### 6.5 Power consumption

Servers may run continuously and consume significant electricity,
particularly when an organization operates many machines.

### 6.6 Cooling requirements

Servers generate heat and need suitable cooling. Poor cooling can cause
overheating, reduced performance, or shutdowns.

### 6.7 Space requirements

Physical servers require space, racks, cabling, power, and suitable
environmental conditions.

### 6.8 Downtime during maintenance

Some repairs or upgrades require a server to be shut down, temporarily
interrupting its services.

### 6.9 Resource underutilization

A server may have more CPU, RAM, or storage capacity than its workload
needs. Some resources can remain unused.

**Example:** A server with 64 GB of RAM might use only 8 GB for a small
application.

### 6.10 Disaster recovery challenges

Recovering from fire, flooding, hardware failure, or another disaster
can be expensive and time-consuming without appropriate backups and
recovery plans.

### 6.11 Quick revision table

  Problem                     Possible impact
  --------------------------- ------------------------------------
  High initial cost           Expensive deployment
  High maintenance            Increased operating expenses
  Hardware failure            Service interruption or data loss
  Limited scalability         Slower or more expensive expansion
  Power consumption           Higher electricity costs
  Cooling requirements        Additional infrastructure costs
  Space requirements          More physical space needed
  Maintenance downtime        Reduced availability
  Resource underutilization   Wasted computing capacity
  Disaster recovery           Complex and costly recovery

## 7. Hardware Virtualization

### 7.1 Definition

**Hardware virtualization** is the creation of virtual machines (VMs) on
a physical machine. Each VM is presented with virtual hardware and can
run its own guest operating system.

A virtualization layer called a **hypervisor** manages the VMs and
allocates physical resources such as CPU time, memory, and storage.

### 7.2 How it works

A simplified hardware virtualization stack is:

``` text
Physical Hardware
       |
       v
Hypervisor
       |
       +------------------+
       |                  |
       v                  v
      VM 1               VM 2
       |                  |
       v                  v
  Guest OS 1         Guest OS 2
       |                  |
       v                  v
 Applications         Applications
```

Each VM runs its own guest OS. The hypervisor manages access to the
underlying physical resources.

> **Important:** The diagram is a conceptual view. In Type 2
> virtualization, a host operating system sits between the physical
> hardware and the hypervisor application.

### 7.3 Example

A single physical server could host:

-   VM 1: A Linux server running a web application.
-   VM 2: A Windows server running a different application.
-   VM 3: Another Linux server used for testing.

Each VM has its own guest OS environment, although the VMs share the
underlying physical machine.

### 7.4 Why use hardware virtualization?

-   Run multiple operating systems on one physical machine.
-   Separate workloads into individual VMs.
-   Use hardware resources more flexibly.
-   Create isolated environments for testing and development.
-   Consolidate multiple server workloads onto fewer physical machines.

## 8. Hypervisors: Type 1 and Type 2

A **hypervisor** is software or a platform layer that creates and
manages virtual machines.

### 8.1 Type 1 hypervisor (bare-metal)

A Type 1 hypervisor runs directly on the physical hardware rather than
as an ordinary application inside a general-purpose host OS.

Examples from the notes:

-   VMware ESXi
-   Microsoft Hyper-V
-   Xen

Simplified layout:

``` text
Virtual Machines
       |
Type 1 Hypervisor
       |
Physical Hardware
```

### 8.2 Type 2 hypervisor (hosted)

A Type 2 hypervisor runs as software on top of a host operating system.

Examples from the notes:

-   Oracle VirtualBox
-   VMware Workstation

Simplified layout:

``` text
Virtual Machines
       |
Type 2 Hypervisor
       |
Host Operating System
       |
Physical Hardware
```

### 8.3 Type 1 vs. Type 2

  -----------------------------------------------------------------------
  Feature                 Type 1                  Type 2
  ----------------------- ----------------------- -----------------------
  Where it runs           Directly on hardware    On top of a host OS

  Typical use             Server and data-center  Desktop labs,
                          virtualization          development, and
                                                  testing

  Example                 VMware ESXi             Oracle VirtualBox
  -----------------------------------------------------------------------

## 9. Problems with Hardware Virtualization

### 9.1 Performance overhead

The virtualization layer adds some overhead compared with running a
workload directly on hardware. The impact varies with the workload,
hardware, and virtualization platform.

### 9.2 Resource contention

Multiple VMs share physical CPU, memory, storage, and I/O resources.
Heavy workloads can compete for those resources and affect performance.

### 9.3 Complex management

Managing many VMs, virtual networks, storage, and OS installations can
require specialized tools and skills.

### 9.4 Security risks

A vulnerability in a hypervisor or its configuration can create security
risks. In some circumstances, an attacker might attempt to escape a VM's
isolation and affect the host or other VMs.

### 9.5 Licensing and costs

Some virtualization platforms require paid licenses. Hardware and
infrastructure costs may also offset some savings.

### 9.6 Backup and recovery complexity

VM backups and recovery need careful planning to preserve data
consistency and meet recovery requirements.

### 9.7 Hardware compatibility problems

Some platforms may not support every device, peripheral, or specialized
hardware configuration.

### 9.8 Single point of failure

If a physical host fails, the VMs running on it may become unavailable.
High-availability designs can reduce this risk but add complexity and
cost.

### 9.9 Hypervisor overhead and maintenance

Hypervisors require updates and security patches. Maintenance must be
planned to reduce disruption and keep the platform secure.

### 9.10 Scalability limits

As the number or size of VMs grows, the host may run short of CPU,
memory, storage, or network capacity. More hardware or better resource
allocation may be needed.

### 9.11 Latency in real-time applications

Virtualization can introduce latency or variability that may be
unsuitable for some latency-sensitive workloads. Suitability depends on
the platform and configuration.

### 9.12 Skill and knowledge requirements

Administrators need knowledge of virtualization, operating systems,
networking, storage, security, and troubleshooting.

## 10. OS Virtualization: Containerization

### 10.1 Definition

**OS virtualization**, commonly associated with containerization, allows
multiple isolated user-space environments called **containers** to run
on one host OS while sharing the same OS kernel.

A container packages an application and the files, libraries, and
configuration it needs. It does not normally include a separate full
guest operating system kernel.

### 10.2 How it works

A simplified container stack is:

``` text
+---------------------+  +---------------------+
|     Container 1     |  |     Container 2     |
|    Application A    |  |    Application B    |
| Required libraries  |  | Required libraries  |
+---------------------+  +---------------------+
             \                 /
              \               /
               Container Engine
                       |
                Host OS Kernel
                       |
                Physical Hardware
```

The container engine creates and manages containers. The host kernel
provides the underlying OS services, while containers are isolated from
one another through operating-system features.

### 10.3 What is a container engine?

A container engine is software that creates and runs containers.

Examples:

-   **Docker**
-   **Podman**

**Kubernetes** is different: it is a container orchestration platform
used to deploy and manage containerized workloads across machines. It is
not simply another container engine.

### 10.4 What does a container contain?

A container image commonly includes:

-   Application code
-   Required runtime
-   Libraries and dependencies
-   Application files and configuration defaults

The container uses the host kernel rather than packaging a separate
kernel inside each container.

### 10.5 Example

Suppose you need to run two web applications:

-   Container 1 runs a Python web application.
-   Container 2 runs a Node.js web application.

Each container can have its own application files and dependencies. Both
can run on the same compatible host OS kernel without requiring a
separate full guest OS for each application.

## 11. Advantages and Use Cases of Containers

### 11.1 Lightweight

Containers generally use fewer resources than VMs because they share the
host OS kernel rather than each running a separate guest OS.

### 11.2 Fast startup

Containers often start faster than VMs because they do not need to boot
a full guest operating system.

### 11.3 Efficient resource usage

Containers can allow multiple applications to share a host efficiently,
although actual resource usage depends on their workloads and limits.

### 11.4 Consistent application packaging

An application image can package the application and its dependencies,
helping reduce differences between development, testing, and deployment
environments.

### 11.5 Common use cases

1.  **Microservices:** Run different application components in separate
    containers.
2.  **Application development and testing:** Create repeatable
    environments for testing software.
3.  **CI/CD:** Use containers as part of automated build, test, and
    deployment workflows.
4.  **Lightweight application deployment:** Package and deploy
    applications with their dependencies.

### 11.6 Important limitations

-   Containers share the host kernel, so they do not provide exactly the
    same isolation boundary as VMs.
-   A container still depends on the underlying physical or virtual
    machine.
-   Containers do not eliminate hardware failures, power requirements,
    or cooling needs.
-   Container security still requires appropriate configuration, access
    controls, image management, and updates.

## 12. Hardware Virtualization vs. OS Virtualization

  -----------------------------------------------------------------------
  Feature                 Hardware Virtualization OS Virtualization
                          (VMs)                   (Containers)
  ----------------------- ----------------------- -----------------------
  Definition              Virtualizes hardware to Isolates multiple
                          run multiple VMs        user-space environments
                                                  while sharing the host
                                                  kernel

  Virtualization layer    Hypervisor              Container engine and OS
                                                  isolation features

  Operating system        Each VM runs its own    Containers share the
                          guest OS                host OS kernel

  Isolation               Separate guest OS       Isolated user spaces
                          environments; strength  that share the kernel
                          depends on platform and 
                          configuration           

  Resource overhead       Usually higher because  Usually lower because
                          each VM runs a guest OS containers share the
                                                  kernel

  Startup time            Usually longer because  Usually shorter because
                          a guest OS must boot    container processes
                                                  start without booting a
                                                  separate OS

  OS flexibility          Can run different guest Requires compatibility
                          OS types supported by   with the host kernel;
                          the hypervisor and      containers do not each
                          hardware                bring their own kernel

  Performance             Includes virtualization Often efficient for
                          overhead; varies by     application workloads;
                          workload                varies by workload

  Security                VM boundaries provide a Kernel is shared, so
                          separate guest OS       kernel-level
                          environment             vulnerabilities can
                                                  affect the host and
                                                  potentially other
                                                  containers

  Resource utilization    Guest OS instances      Often higher density
                          consume additional      for compatible
                          resources               workloads

  Management              Requires VM and guest   Requires image,
                          OS management           container, and often
                                                  orchestration
                                                  management

  Common use cases        Multiple OS types,      Microservices,
                          server consolidation,   application packaging,
                          legacy applications,    CI/CD, lightweight
                          OS-level testing        deployment

  Examples                VMware ESXi, Hyper-V,   Docker, Podman, LXC
                          KVM, VirtualBox         
  -----------------------------------------------------------------------

### 12.1 Easy example

Imagine one physical server:

**Using VMs:** - Install a hypervisor. - Create a Linux VM and a Windows
VM. - Each VM runs its own guest OS. - Install applications inside the
relevant VM.

**Using containers:** - Use a compatible host OS. - Run a container
engine. - Start one container for a web application and another for a
database. - Each container has its own application environment, but both
share the host kernel.

### 12.2 Which should you choose?

Choose **VMs** when you need separate guest operating systems or a VM
boundary for different workloads.

Choose **containers** when you want to package, deploy, and scale
applications efficiently on a compatible OS platform.

VMs and containers can also be used together. For example, a container
engine can run inside a Linux VM.

## 13. How Docker Helps

Docker helps package and run applications in containers. It can make
application deployment more repeatable and improve resource utilization
when several applications share a host.

Docker can help teams:

-   Package applications and dependencies into images.
-   Run applications in isolated containers.
-   Create repeatable development and testing environments.
-   Deploy applications across compatible environments.
-   Manage container lifecycles through Docker tooling.

However, Docker does **not** remove the need for physical
infrastructure. Containers still depend on a host machine, its operating
system kernel, CPU, memory, storage, network, power, and cooling.

### Simple Docker mental model

-   **Image:** A template used to create containers.
-   **Container:** A running or stopped instance created from an image.
-   **Docker Engine:** The software that builds and runs containers.
-   **Host:** The machine and OS environment that provide the resources.

## 14. Quick Revision

### Key definitions

-   **Physical server:** A real machine that provides computing,
    storage, and networking services.
-   **Hypervisor:** A platform layer that creates and manages virtual
    machines.
-   **Virtual machine:** A software-based computer with virtual hardware
    and its own guest OS.
-   **Container:** An isolated application environment that shares the
    host OS kernel.
-   **Container engine:** Software that creates and runs containers.
-   **Kubernetes:** A platform for orchestrating containerized
    workloads.

### Remember these points

1.  A physical server is the hardware foundation.
2.  A VM has its own guest operating system.
3.  Type 1 hypervisors run directly on hardware.
4.  Type 2 hypervisors run on a host OS.
5.  Containers share the host OS kernel.
6.  Containers generally start faster and use fewer resources than VMs
    for comparable application workloads.
7.  VMs and containers have different isolation and OS-flexibility
    characteristics.
8.  Docker runs containers; Kubernetes orchestrates containerized
    workloads.
9.  Virtualization and containers can improve resource use, but neither
    eliminates hardware failures.
10. The right choice depends on the application, OS requirements,
    security needs, and operational constraints.

## 15. Practice Questions

### Short-answer questions

1.  What is a physical server?
2.  What are the main components of a physical server?
3.  What is the purpose of the CPU?
4.  What is the difference between RAM and storage?
5.  What is a hypervisor?
6.  Differentiate Type 1 and Type 2 hypervisors.
7.  What is a virtual machine?
8.  What is OS virtualization?
9.  What is a container engine?
10. What is the difference between Docker and Kubernetes?

### Understanding questions

1.  Why can physical servers suffer from resource underutilization?
2.  How does a hypervisor allow multiple VMs to share a physical server?
3.  Why do VMs generally require more resources than containers?
4.  Why can containers start faster than VMs?
5.  Why does sharing a host kernel matter for container isolation?
6.  Can containers eliminate physical server failures? Explain.
7.  When might a VM be a better choice than a container?
8.  Can containers run inside a VM? Give an example.

### Suggested answers

1.  A physical server is a real computer that provides resources and
    services to clients or applications.
2.  Common components include the CPU, RAM, storage, NIC, motherboard,
    PSU, and cooling system.
3.  The CPU executes instructions and processes workloads.
4.  RAM temporarily holds data in use; storage retains data
    persistently.
5.  A hypervisor creates and manages VMs and allocates virtualized
    access to physical resources.
6.  Type 1 runs directly on hardware; Type 2 runs on top of a host OS.
7.  A VM is a software-based computer with virtual hardware and its own
    guest OS.
8.  OS virtualization runs isolated user-space environments that share
    the host kernel.
9.  A container engine creates and runs containers.
10. Docker provides tools for building and running containers;
    Kubernetes orchestrates containerized workloads.
11. A server may have more CPU, RAM, or storage than its workload needs,
    leaving some capacity unused.
12. The hypervisor manages VM execution and allocates physical resources
    to them.
13. Each VM normally runs a separate guest OS, while containers share
    the host kernel.
14. A container does not need to boot a separate guest OS.
15. Containers share the host kernel, so their isolation differs from
    separate guest OS environments.
16. No. Containers still depend on the underlying machine and
    infrastructure.
17. A VM may be preferable when a workload needs a different supported
    guest OS or a separate guest OS environment.
18. Yes. For example, a Linux VM can run Docker and host several
    application containers.

------------------------------------------------------------------------

## Final Summary

Physical servers provide the hardware resources needed to run workloads.
Hardware virtualization uses a hypervisor to run multiple VMs, each with
its own guest OS. OS virtualization uses containers to isolate
applications while sharing the host kernel. Containers are generally
lightweight and quick to start, while VMs offer separate guest OS
environments. Docker helps build and run containers, and Kubernetes
helps orchestrate them at scale.
