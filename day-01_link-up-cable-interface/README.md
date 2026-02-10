# Lab Day 01 — Link Up: Cable & Interface State (Host ↔ Switch)

## Objective Mapping
- Network+ 1.5 Transmission Media and Transceivers
- Network+ 5.2 Cabling & Interface Issues
- Network+ 5.5 Tools (ipconfig, ping)

## Core Primitives Exercised
Node, Host, Workstation, NIC, Interface, Medium, Link, Physical Layer, Data Link Layer, Topology, Baseline, ping

## Topology Description
Single host connected to an access switch in a star topology to isolate Layer 1 behavior.

## IP Plan
| Device | Interface | IP Address     | Subnet Mask     | Gateway |
|--------|-----------|----------------|------------------|---------|
| PC-PT  | Fa0       | 192.168.1.10   | 255.255.255.0    | (none)  |

## Build Summary
1. Place PC-PT and 2960 switch.
2. Connect PC Fa0 to Switch Fa0/1 using copper Ethernet.
3. Assign static IPv4 to PC.
4. Verify link up and local ping.

## Verification
- `ipconfig` confirms static IP (Layer 3 config present).
- `ping 192.168.1.10` confirms NIC, interface, medium, and link are operational.
- Layer mapping: ICMP depends on L1/L2 viability.

## Failure Injection
**What was broken:** PC FastEthernet0 interface set to Port Status: Off.  
**Symptoms:** Link down; ICMP timeouts (100% loss).  
**Diagnosis:**  
- Layer: Physical (L1 manifestation)  
- Scope: Local host interface / single link  
- Primitive: Interface (admin down) → NIC prevented from signaling  
**Fix:** Re-enable interface (Port Status: On).

## Changelog
- New concept added vs. prior lab: None (Lab Day 1 baseline).

## What I Learned
- **Micro:** An administratively down interface blocks NIC signaling, causing a link-down condition even with correct IP settings.
- **Macro:** Modern Ethernet may auto-correct cable types; interface state remains a reliable L1 failure lever.
- **Troubleshooting:** Bottom-up verification—check link/interface state before addressing IP or services.

