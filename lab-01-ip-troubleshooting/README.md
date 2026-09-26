# Lab 1 — Troubleshooting Basic LAN Connectivity

## Overview

This lab was created in Cisco Packet Tracer to practice basic IP addressing, connectivity testing, and troubleshooting.

## Network Topology

The lab uses two PCs connected to a Layer 2 switch.

**Devices:**

* 2 × PCs
* 1 × Layer 2 switch
* Copper straight-through Ethernet cables

## Initial Configuration

| Device | IP Address     | Subnet Mask     |
| ------ | -------------- | --------------- |
| PC1    | `192.168.1.10` | `255.255.255.0` |
| PC2    | `192.168.1.20` | `255.255.255.0` |

With both PCs on the same IP network, connectivity was successfully tested using `ping`.

## Troubleshooting Scenario

To simulate an IP configuration problem, I changed PC2's IP address to:

`192.168.2.20`

The subnet mask remained:

`255.255.255.0`

I then tested connectivity from PC1 using:

```text
ping 192.168.2.20
```

### Result

The ping failed with:

* Packets sent: 4
* Packets received: 0
* Packet loss: 100%

## Investigation

I checked the IP configuration of both PCs and compared their network addresses.

* PC1: `192.168.1.10/24`
* PC2: `192.168.2.20/24`

Because the subnet mask was `/24`, the PCs were on different IP networks:

* PC1 → `192.168.1.0/24`
* PC2 → `192.168.2.0/24`

The lab only contained a Layer 2 switch, so there was no routing device to provide communication between the two networks.

## Resolution

I restored PC2's IP address to:

`192.168.1.20`

I then repeated the ping test from PC1.

The ping was successful, confirming that connectivity had been restored.

## Troubleshooting Process

This lab followed a basic troubleshooting process:

**Test → Check configuration → Compare → Identify the cause → Correct → Retest**

## What I Learned

* How to verify connectivity using `ping`
* How to identify the network portion of an IPv4 address using a subnet mask
* How an incorrect IP configuration can prevent communication
* The difference between switching within a local network and routing between different networks
* The importance of retesting after applying a fix

## Evidence

* `topology.png` — Packet Tracer network topology
* `failed-ping.png` — Failed connectivity test showing 100% packet loss
* `ip-troubleshooting.pkt` — Cisco Packet Tracer project file
