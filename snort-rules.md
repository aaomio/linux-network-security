# Snort Rule Configuration

This section configures Snort rules to detect network activity generated between VM1 (IDS) and VM2 (client).

The goal is to create basic detection rules for ICMP and TCP traffic within the 192.168.1.0/24 network.

---

## Default Rule File Location

Snort rules are stored in:

```text
/etc/snort/rules/
```

Main rule file:

```text
local.rules
```

---

## Edit Local Rules File

Open the file:

```bash
sudo nano /etc/snort/rules/local.rules
```

---

## Add ICMP Detection Rule

Add the following rule to detect ICMP (ping) traffic:

```text
alert icmp any any -> 192.168.1.0/24 any (msg:"ICMP Packet Detected"; sid:1000001; rev:1;)
```

---

## Add TCP Detection Rule (Port Scan Simulation)

Add a rule to detect TCP traffic:

```text
alert tcp any any -> 192.168.1.0/24 any (msg:"TCP Traffic Detected"; sid:1000002; rev:1;)
```

---

## Save and Exit

Save file:

```text
CTRL + O
```

Exit:

```text
CTRL + X
```

---

## Enable Local Rules in Snort Config

Edit Snort configuration file:

```bash
sudo nano /etc/snort/snort.conf
```

Ensure the following line is enabled (not commented):

```text
include $RULE_PATH/local.rules
```

---

## Validate Configuration

Run Snort in test mode:

```bash
sudo snort -T -c /etc/snort/snort.conf
```

Expected output:

```text
Snort successfully validated the configuration
```

---

## Test Rule Triggering

Start Snort in IDS mode:

```bash
sudo snort -A console -i enp0s8 -c /etc/snort/snort.conf
```

From VM2, generate traffic:

```bash
ping -c 5 192.168.1.1
```

---

## Expected Result

On VM1 (Snort), alerts should appear:

```text
ICMP Packet Detected
```

