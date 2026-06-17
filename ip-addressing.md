# Static IP Configuration

This section configures static IPv4 addressing for both virtual machines on the isolated Internal Network.

The lab network uses the following subnet:

```text
Network:    192.168.1.0/24
Broadcast:  192.168.1.255
```

---

## VM1

Bring the interface down before assigning the address.

Run:

```bash
sudo ifconfig enp0s8 down
```

Assign the static IP address.

Run:

```bash
sudo ifconfig enp0s8 192.168.1.1 netmask 255.255.255.0 broadcast 192.168.1.255
```

Bring the interface back online.

Run:

```bash
sudo ifconfig enp0s8 up
```

Verify the configuration.

Run:

```bash
ip addr show enp0s8
```

Expected address:

```text
192.168.1.1/24
```

---

## VM2

Assign the second address on the same subnet.

Run:

```bash
sudo ifconfig enp0s8 down
```

```bash
sudo ifconfig enp0s8 192.168.1.2 netmask 255.255.255.0 broadcast 192.168.1.255
```

```bash
sudo ifconfig enp0s8 up
```

Verify the configuration.

Run:

```bash
ip addr show enp0s8
```

Expected address:

```text
192.168.1.2/24
```

---

## Verify Connectivity

From VM1, test communication with VM2.

Run:

```bash
ping 192.168.1.2
```

From VM2, test communication with VM1.

Run:

```bash
ping 192.168.1.1
```

Expected output:

```text
64 bytes from 192.168.1.x: icmp_seq=1 ttl=64 time=...
```

Stop the test with:

```text
Ctrl + C
```

---

## Verify Interface Configuration

Display the configured interface.

Run:

```bash
ifconfig enp0s8
```

or

```bash
ip addr show enp0s8
```

Confirm:

```text
VM1
192.168.1.1

VM2
192.168.1.2

Netmask
255.255.255.0

Broadcast
192.168.1.255
```

The isolated network is now configured and ready for Snort installation.