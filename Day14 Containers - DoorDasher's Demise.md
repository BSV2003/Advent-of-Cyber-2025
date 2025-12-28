# Containers - DoorDasher's Demise
Continue your Advent of Cyber journey and learn about container security.

## Learning Objectives
- Learn how containers and Docker work, including images, layers, and the container engine
- Explore Docker runtime concepts (sockets, daemon API) and common container escape/privilege-escalation vectors
- Apply these skills to investigate image layers, escape a container, escalate privileges, and restore the DoorDasher service
- DO NOT order “Santa's Beard Pasta”

**Containers:** Containers are packages of software that bundles up code, and all its dependencies so it can be run reliably in any environment.

---

# What Are Containers?

Modern applications face several challenges:
- **Installation issues** due to environment differences
- **Troubleshooting complexity** when apps fail
- **Dependency conflicts** between different app versions

**Containerisation** solves these problems by packaging:
- The application
- All its dependencies
- Runtime configuration  

…into a single isolated unit called a **container**.

## Key Benefits
- Lightweight
- Fast startup
- Consistent across environments
- Easy to scale

---

# Containers vs Virtual Machines (VMs)

## Virtual Machines (VMs)
- Run on a **hypervisor**
- Include a **full guest OS**
- Heavy but strongly isolated
- Ideal for running different operating systems

## Containers
- Share the **host OS kernel**
- Isolate only the application and dependencies
- Lightweight and fast
- Ideal for **microservices** and scalable apps

<img width="1015" height="536" alt="image" src="https://github.com/user-attachments/assets/d95743ae-37d0-4ad0-9e49-144444dee537" />

**OS:** Operating System (OS) is a layer between the hardware and the applications. From the application's perspective, the OS provides an interface to access the different hardware components, such as CPU, RAM, and disk storage. Examples of OS are Android, FreeBSD, Linux, macOS, and Windows.

---

# Applications at Scale (Microservices)

Modern applications are often split into **microservices** instead of one monolithic app.

## Why Microservices?
- Scale only what’s needed
- Improve resilience
- Faster deployments

Containers are perfect for this architecture due to their:
- Speed
- Portability
- Resource efficiency

---

# Container Engines & Docker

A **container engine** builds, runs, and manages containers using OS features like:
- Namespaces
- cgroups

## Docker
- Open-source container platform
- Uses **Dockerfiles** to define app environments
- Most popular container engine
- Used by DoorDasher in this challenge

---

# Container Escape & Docker Sockets

A **container escape** occurs when code inside a container gains access beyond its isolation, such as:
- Host system
- Other containers

## Docker Socket Risk
Docker uses a client-server model:
- Docker CLI → Docker Daemon
- Communication happens via a **Unix socket**:
  /var/run/docker.sock
⚠️ If a container can access this socket, it can control Docker itself — a **critical security risk**.

---

# Challenge Overview

Your goal:
> Investigate Docker containers and restore the defaced **Hopperoo** website back to **DoorDasher**.

## Enumeration

List running containers:
```bash
docker ps
```
You should see multiple services running.

The main web service runs at:
```cpp
http://10.81.139.45:5001
```
The site has been defaced to Hopperoo.

## Container Access

Access the uptime-checker container:
```bash
docker exec -it uptime-checker sh
```

Check Docker socket access:
```bash
ls -la /var/run/docker.sock
```

If accessible, attempt Docker commands:
```bash
docker ps
```
✅ Successful execution confirms a Docker escape opportunity.

> Docker normally blocks containers from accessing the Docker socket to prevent abuse. When containers are allowed to access it (usually for testing or administration), they can control Docker through its API. If misused, this can let attackers escape the container and affect the host system.

## Privileged Container Access

Access the deployer container:
```bash
docker exec -it deployer bash
```

Check current user:
```bash
whoami
```
Explore the container filesystem to locate the recovery script.

## Restoring the Service

Run the recovery script:
```bash
sudo /recovery_script.sh
```

Refresh the site:
```cpp
http://10.81.139.45:5001
```

---

**What exact command lists running Docker containers?**
`docker ps`

**What file is used to define the instructions for building a Docker image?**
`Dockerfile`
