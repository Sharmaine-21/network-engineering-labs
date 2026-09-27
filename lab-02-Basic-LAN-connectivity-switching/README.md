# Lab 1 — Basic LAN Connectivity and Switching

## Overview

This lab was created in Cisco Packet Tracer to practice basic IP addressing, connectivity testing, and Layer 2 switching.

## Network Topology

The lab uses two PCs connected to a Layer 2 switch.

**Devices:**

* 2 × PCs
* 1 × Layer 2 switch
* Copper straight-through Ethernet cables

## IP Configuration

| **Device** | **IP Address** | **Subnet Mask** | **Default Gateway** |
| ---------- | -------------- | --------------- | ------------------- |
| PC1        | `192.168.1.10` | `255.255.255.0` | None                |
| PC2        | `192.168.1.20` | `255.255.255.0` | None                |

Both PCs were configured within the same `192.168.1.0/24` network.

## Connectivity Test

Connectivity was tested from PC1 using:

```text
ping 192.168.1.20
```

### Result

The ping was successful:

```text
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
```

This confirmed that PC1 and PC2 could communicate through the switch.

## MAC Address Table

After the connectivity test, the switch's MAC address table was checked using:

```text
enable
show mac address-table
```

The switch showed MAC addresses learned on:

* `Fa0/1`
* `Fa0/2`

This demonstrated that the switch had learned which ports were associated with the connected devices.

## Switching Process

When PC1 communicates with PC2 on the same local network, the Ethernet frame contains a destination MAC address belonging to PC2.

The switch checks its MAC address table and uses the destination MAC address to determine which switch port should forward the frame.

In this lab:

```text
PC1 → Switch → PC2
```

The switch performs Layer 2 forwarding based on MAC addresses.

## Default Gateway

No default gateway was configured for either PC.

This was sufficient because both PCs were on the same IP network:

```text
192.168.1.0/24
```

A default gateway is used when a device needs to communicate with a different IP network.

## What I Learned

* How to configure basic IPv4 addresses in Cisco Packet Tracer
* How to verify IP configuration using `ipconfig`
* How to test connectivity using `ping`
* That devices on the same IP network can communicate through a Layer 2 switch without a default gateway
* The difference between an IP address and a MAC address
* That an Ethernet frame uses source and destination MAC addresses
* How a switch learns MAC addresses and associates them with switch ports
* How to view a switch's MAC address table using `show mac address-table`

## Evidence

* `topology.png` — Packet Tracer network topology
* `pc1-ip-config.png` — PC1 IP configuration
* `pc2-ip-config.png` — PC2 IP configuration
* `ping-success.png` — Successful connectivity test
* `mac-address-table.png` — Switch MAC address table
* `basic-lan.pkt` — Cisco Packet Tracer project file

