---

layout: post
title: "From Virtual Machines to Docker Containers"
description: "How containerisation changed software deployment"
---------------------------------------------------------------

## Docker Images: A Snapshot of an Application

A **Docker Image** is a snapshot of a project that includes:

* Application code
* Dependencies
* Required libraries and frameworks
* Assets
* Configuration
* Startup instructions

An image serves as a **blueprint**. When executed, it becomes a **container**—a runnable instance of that image.

---

## The Old World: Virtual Machines

In the past, hosting a software product was **extremely expensive**.

Each application required a **full Virtual Machine (VM)** to be created on a host machine, running atop a **hypervisor**.

### How VMs Worked

* The hypervisor operates at **kernel space**
* It distinguishes between:

  * **Virtual Machine Monitors (VMMs)** — user space functionality
  * Multiple **guest operating systems**, each fully isolated

Every VM required:

* A full operating system
* Individual installation of all packages and dependencies

### Problems with VM-Based Deployment

* ❌ **High resource overhead**
* ❌ **Inconsistent environments** due to manual dependency installation
* ❌ **Slow scalability** — deploying a new instance meant starting an entire VM
* ❌ **Frequent discrepancies** between environments

---

## The Shift: Docker and Containerisation

Docker introduced **containerisation**, fundamentally changing how software is built and deployed.

Instead of virtualising entire operating systems, Docker enables:

> Packaging multiple modules or parts of a software system into **lightweight, individually runnable environments** called **containers**.

### Containers vs VMs

* Containers share the **host OS kernel**
* No need for a full guest operating system
* Much faster startup time
* Significantly lower resource usage

A container is simply a **running instance of an image**, meaning:

* One image → many containers
* Each container behaves consistently across environments

---

## Scaling at Scale: Container Orchestration

When systems grow, containers are managed using **orchestration platforms**.

### Kubernetes

* Open-source container orchestration system
* Automates:

  * Deployment
  * Scaling
  * Networking
  * Health management

Kubernetes enables **large-scale container management**, making cloud-native architectures possible.

---

## The Impact on Software Development

Containerisation didn’t just improve infrastructure—it transformed **how software is built and released**.

### Before

* Developers worked on isolated parts
* Code was merged only at the **end of development**
* Led to:

  * Messy conflicts
  * Lost time
  * Format and environment discrepancies
* Software releases were **big, infrequent events**

### Now

* Containers enable consistent environments across teams
* CI/CD pipelines become practical and reliable
* Incremental updates can be:

  * Rolled out quickly
  * Tested continuously
  * Deployed safely

This marks a **huge contrast** to traditional release cycles.

---

## Conclusion

Docker and containerisation solved the inefficiencies of VM-based deployment and paved the way for:

* Cloud-native systems
* Scalable microservices
* Modern CI/CD workflows

What once required heavyweight infrastructure can now be achieved with lightweight, reproducible containers.
