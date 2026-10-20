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

## UFW Rules

To allow traffic from a specific IP address or subnet to a destination port, use:

```bash
sudo ufw allow proto <protocol> from <source-ip-or-subnet> to any port <port-number>
```

For example, allow VM2 (`192.168.1.20`) to access SSH on VM1 (TCP port 22):

```bash
sudo ufw allow proto tcp from 192.168.1.20 to any port 22
```

Allow the entire lab subnet to access a web server on TCP port 80:

```bash
sudo ufw allow proto tcp from 192.168.1.0/24 to any port 80
```

Allow the lab subnet to access HTTPS on TCP port 443:

```bash
sudo ufw allow proto tcp from 192.168.1.0/24 to any port 443
```

Allow UDP traffic from a specific source to port 53:

```bash
sudo ufw allow proto udp from 192.168.1.20 to any port 53
```

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
