# Lab 3 — Basic Routing Between Two Networks

## Overview

This lab demonstrates basic routing between two different IP networks using a Cisco 1941 router.

I built a network with two PCs, two Cisco 2960 switches, and one router. I configured the router interfaces and tested communication between the two networks.

I also created a troubleshooting scenario by changing the IP address of one router interface and then investigated the resulting connectivity problem.

## Network Topology

```text
PC1 ── Switch 1 ── Router ── Switch 2 ── PC2

```

More specifically:

```text
PC1 ── Fa0/2 ── Switch 1 ── Fa0/1 ── G0/0 Router G0/1 ── Fa0/1 ── Switch 2 ── Fa0/2 ── PC2
```

## Devices

* 2 × PCs
* 2 × Cisco 2960 switches
* 1 × Cisco 1941 router
* Copper Straight-Through cables

## IP Addressing

### Network 1

| Device | Interface | IP Address     | Subnet Mask     | Default Gateway |
| ------ | --------- | -------------- | --------------- | --------------- |
| PC1    | Fa0       | `192.168.1.10` | `255.255.255.0` | `192.168.1.1`   |
| Router | G0/0      | `192.168.1.1`  | `255.255.255.0` | —               |

### Network 2

| Device | Interface | IP Address     | Subnet Mask     | Default Gateway |
| ------ | --------- | -------------- | --------------- | --------------- |
| Router | G0/1      | `192.168.2.1`  | `255.255.255.0` | —               |
| PC2    | Fa0       | `192.168.2.10` | `255.255.255.0` | `192.168.2.1`   |

## Router Configuration

I configured the router interfaces with the following commands:

```text
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
```

The router's interfaces were then:

```text
G0/0 = 192.168.1.1
G0/1 = 192.168.2.1
```

## Connectivity Test

From PC1, I tested connectivity to PC2:

```text
ping 192.168.2.10
```

The ping was successful.

This confirmed that the router was able to forward traffic between the two different networks.

## Troubleshooting Scenario

To practice troubleshooting, I intentionally changed the IP address of the router's G0/1 interface.

Original configuration:

```text
G0/1 = 192.168.2.1
```

Changed to:

```text
G0/1 = 192.168.3.1
```

PC2 remained configured as:

```text
IP Address:       192.168.2.10
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.2.1
```

I then tested the connection again from PC1:

```text
ping 192.168.2.10
```

The ping failed with an unreachable message.

## Investigation

I checked the router interfaces using:

```text
show ip interface brief
```

The result showed:

```text
GigabitEthernet0/0     192.168.1.1     up     up
GigabitEthernet0/1     192.168.3.1     up     up
```

The problem was that the router's G0/1 interface was configured with an IP address belonging to the `192.168.3.0/24` network, while PC2 was still on the `192.168.2.0/24` network.

Because of this mismatch, the router was no longer directly connected to the `192.168.2.0/24` network through G0/1.

## Resolution

I restored the router's G0/1 interface:

```text
interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
```

I then tested connectivity again:

```text
ping 192.168.2.10
```

The ping was successful.

## What I Learned

* A router connects different IP networks.
* Each router interface can belong to a different IP network.
* The router interface on the local network can serve as the PC's default gateway.
* A PC's default gateway should point to the router interface on the same subnet.
* `show ip interface brief` is useful for checking router interface IP addresses and status.
* An incorrect router interface IP address can cause communication between networks to fail.
* `ping` can be used to verify connectivity before and after troubleshooting.

## Evidence

* `topology.png` — Network topology
* `successful-ping.png` — Successful PC1-to-PC2 ping
* `failed-ping.png` — Failed ping during troubleshooting
* `show-ip-interface-brief01.png` — Router interface verification
* `show-ip-interface-brief02.png` — Router interface verification
* `basic-routing.pkt` — Packet Tracer project file

