# Firewall Configuration

This section configures the Linux firewall on VM1 (Snort IDS) to control and monitor network traffic between VM1 and VM2.

The firewall works alongside Snort to provide layered network security visibility.

---

## Check Current Firewall Status

Run:

```bash
sudo ufw status
```

If inactive, enable it:

```bash
sudo ufw enable
```

---

## Allow SSH Access 

If using SSH for remote access:

```bash
sudo ufw allow ssh
```

---

## Allow ICMP Traffic (Ping)

By default, ICMP is usually allowed, but verify rule behaviour using Snort.

To explicitly allow ICMP:

```bash
sudo ufw allow proto icmp
```

---

## Allow Internal Network Traffic

Allow all traffic within the lab subnet:

```bash
sudo ufw allow from 192.168.1.0/24
```

This ensures VM1 and VM2 can communicate for testing IDS rules.

---

## View Active Rules

Run:

```bash
sudo ufw status verbose
```

Expected output includes:

```text
192.168.1.0/24 ALLOW
```

---

## Test Firewall + Snort Interaction

From VM2, generate traffic:

```bash
ping -c 5 192.168.1.1
```

On VM1:

- Firewall allows traffic based on rules
- Snort logs and alerts ICMP packets

---

## Removing a Firewall Rule (UFW)

If a firewall rule needs to be removed, first list rules:

```bash
sudo ufw status numbered
```

Example output:

```text
[ 1] 192.168.1.0/24 ALLOW IN
[ 2] ssh ALLOW IN
```

---

## Delete a Rule 

Remove a specific rule:

```bash
sudo ufw delete 1
```

Alternatively:

```bash
sudo ufw delete allow from 192.168.1.0/24
```

---

## Verify Removal

Check updated rules:

```bash
sudo ufw status verbose
```

# Viewing UFW Logs

UFW logs are stored in `/var/log/ufw.log`. To view them in real time, use:

```bash
sudo tail -f /var/log/ufw.log
```

To display the full log file:

```bash
sudo cat /var/log/ufw.log
```

To clear (truncate) the log file for fresh testing:

```bash
sudo truncate -s 0 /var/log/ufw.log
```

This resets the log file to zero bytes without deleting it.
