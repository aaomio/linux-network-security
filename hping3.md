# Traffic Generation with hping3

This section uses hping3 to generate custom network traffic and simulate basic attack patterns against VM1 (Snort IDS).

The tool allows controlled testing of firewall and IDS detection rules.

---

## Install hping3

On VM2 (attack client), install hping3:

```bash
sudo apt update
sudo apt install hping3 -y
```

---

## Basic ICMP Test

Send ICMP packets to VM1:

```bash
hping3 -1 192.168.1.1
```

To send a limited number of packets:

```bash
hping3 -1 -c 5 192.168.1.1
```

---

## TCP SYN Scan Simulation

Send SYN packets to port 80:

```bash
hping3 -S -p 80 -c 5 192.168.1.1
```

---

## UDP Traffic Test

Send UDP packets:

```bash
hping3 --udp -p 53 -c 5 192.168.1.1
```

---

## Flood Test (Controlled)

Simulate traffic flood (use carefully in lab only):

```bash
hping3 -1 --flood 192.168.1.1
```

Stop with:

```text
CTRL + C
```

---

## Snort Detection Verification

On VM1 (Snort IDS), run:

```bash
sudo snort -A console -i enp0s8 -c /etc/snort/snort.conf
```

Expected:

- ICMP alerts
- TCP SYN alerts
- UDP traffic alerts

## Spoofed Payload Injection with hping3

This command sends a single crafted TCP packet with a custom payload while spoofing the source IP address for IDS testing.

```bash
sudo hping3 -c 1 -p 80 -s 1234 -a 10.10.10.10 -E pcap1_payload -d 720 192.168.1.1
```

---

## Parameter Breakdown

### `sudo`
Runs the command with administrative privileges required for packet crafting.

### `hping3`
Tool used for generating custom TCP/IP packets.

### `-c 1`
Sends **1 packet only**.

### `-p 80`
Sets destination port to **80 (HTTP)**.

### `-s 1234`
Sets source port to **1234**.

### `-a 10.10.10.10`
Spoofs the source IP address, making the packet appear as if it originated from `10.10.10.10`.

### `-E pcap1_payload`
Attaches a custom payload file to the packet.

### `-d 720`
Sets the payload size to **720 bytes**.

### `192.168.1.1`
Target IP address (VM1 running Snort IDS).

