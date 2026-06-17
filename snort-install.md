# Snort Installation

This section installs and configures Snort on VM1 to monitor network traffic between the two VirtualBox machines.

Snort will act as an Intrusion Detection System (IDS) to detect ICMP, TCP, and UDP traffic generated from VM2.

---

## Update System Packages

Run:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Install Snort

Run:

```bash
sudo apt install snort -y
```

During installation:

- Select the correct network interface when prompted  
- Choose:

```text
enp0s8
```

(This is the Internal Network interface)

---

## Verify Installation

Check Snort version:

```bash
snort -V
```

Expected output:

```text
Snort version 2.x.x or 3.x.x
```

---

## Confirm Network Interface

Run:

```bash
ip addr
```

Ensure Snort will monitor:

```text
192.168.1.1 (VM1 interface)
```

---

## Test Snort in Console Mode

Run Snort in packet sniffing mode:

```bash
sudo snort -i enp0s8 -v
```

This will:

- Capture live traffic
- Display packets in terminal
- Confirm Snort is receiving network data

---

## Test Connectivity

From VM2, generate traffic:

```bash
ping -c 5 192.168.1.1
```

On VM1, Snort should display ICMP packets.

