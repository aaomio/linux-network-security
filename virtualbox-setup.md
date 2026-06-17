# VirtualBox Setup

This section creates an isolated network using Oracle VM VirtualBox for the Snort IDS lab.

The lab consists of two Linux virtual machines connected using an Internal Network, allowing traffic to be generated and analysed without affecting the host network.

---

## Lab Topology

```text
               Internal Network

     192.168.1.0/24

+----------------+        +----------------+
|      VM1       |        |      VM2       |
|----------------|        |----------------|
| Snort IDS      |<------>| Attack Client  |
| 192.168.1.1    |        | 192.168.1.2    |
+----------------+        +----------------+
```

---

## Software Requirements

* Oracle VM VirtualBox
* Two Linux virtual machines (Ubuntu Server recommended)

---

## Create the Virtual Machines

Create two virtual machines.

Example:

```text
VM1
Ubuntu Server

VM2
Ubuntu Server
```

---

## Configure Network Adapter

For both virtual machines:

Open:

```text
Settings
Network
Adapter 2
```

Configure:

```text
Enable Network Adapter

Attached to:
Internal Network

Name:
InternalNetwork
```

Adapter 1 may remain configured as NAT to provide Internet access for installing packages.

Adapter 2 will be used exclusively for communication between the virtual machines.

---

## Verify Adapter Names

After booting each virtual machine, identify the network interfaces.

Run:

```bash
ip addr
```

or

```bash
ifconfig
```

The internal adapter will typically appear as:

```text
enp0s8
```

At this stage, both virtual machines should detect the Internal Network adapter but will not yet communicate until static IP addresses have been assigned.