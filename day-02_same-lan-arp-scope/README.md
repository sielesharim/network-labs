# Lab Day 02 — Same LAN, ARP Scope & Subnet Logic

## Objective
Demonstrate how IP subnet membership determines whether ARP is used for local delivery, even when hosts are physically connected to the same switch.

---

## Network+ Objectives
- 1.7 IPv4 Addressing
- 1.1 OSI Model (Layer 2 vs Layer 3 responsibility)
- 5.0 Network Troubleshooting (scope isolation)

---

## Core Primitives Exercised
- Host
- LAN
- IP Address
- Subnet
- Addressing
- ARP
- Switch
- Baseline

---

## Topology
- 2 × PCs
- 1 × Layer 2 switch
- Star topology (access layer)

See `topology.png`.

---

## IP Plan

| Device | Interface | IPv4 Address | Subnet Mask |
|------|----------|--------------|-------------|
| PC0 | FastEthernet0 | 192.168.1.10 | 255.255.255.0 |
| PC1 | FastEthernet0 | 192.168.1.11 | 255.255.255.0 |

Failure state:
- PC1 temporarily assigned `192.168.2.11 /24`

---

## Build Summary
1. Place PC0 and PC1.
2. Connect both to the switch using copper straight-through cables.
3. Configure static IPv4 addresses.
4. Verify baseline connectivity.

---

## Verification (Baseline)
Commands executed from PC0:
- `ping 192.168.1.11`
- `arp -a`

Expected:
- Ping succeeds.
- ARP table populated with PC1 MAC address.

Evidence:
- `evidence/ping-success.txt`
- `evidence/arp-success.txt`

---

## Failure Injection
**Injected fault:** PC1 placed in a different subnet (`192.168.2.11 /24`).

---

## Observed Symptoms
- Ping fails with 100% packet loss.
- ARP table remains empty.
- Physical link remains up.

Evidence:
- `evidence/ping-failure-different-subnet.txt`
- `evidence/arp-empty-after-failure.txt`

---

## Diagnosis

| Category | Result |
|------|------|
| Failure Layer | Layer 3 (Network) |
| Scope Violated | Subnet |
| Why ARP Failed | Destination classified as non-local |
| Switch Role | Forwarding only, not causal |

---

## Corrective Action
Restore both hosts to the same subnet.

No changes required at:
- Layer 1
- Layer 2
- Switch configuration

---

## What I Learned

### Micro Insight
ARP only operates when a destination IP is classified as local by subnet logic.

### Macro Insight
Physical proximity does not imply logical reachability.

### Troubleshooting Insight
Ping failure + empty ARP + link up strongly indicates Layer 3 scope mismatch.

---

## Changelog
- Introduced ARP scope and subnet-based delivery logic.

