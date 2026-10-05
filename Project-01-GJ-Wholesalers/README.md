# Project 1 – GJ Wholesalers Ltd: Branch Network

## Scenario

GJ Wholesalers Ltd is an electrical wholesaler opening a new trade branch in Leicestershire. The site has a back office, a customer-facing trade counter and a stock warehouse. The branch runs independently of head office and needs a secure, segmented network with voice, wireless, CCTV and internet access, delivered from a single wall-mounted comms cabinet.

This project covers the full lifecycle: **design → physical build documentation → configuration → verification → break/fix**.

> Built in Cisco Packet Tracer. All passwords and keys are lab-only values and are not used anywhere else.

---

## Requirements

1. One Cisco router and one Cisco switch, housed in a wall-mounted comms cabinet.
2. Office, Trade Counter and Warehouse each in their own VLAN.
3. IP phones on a dedicated voice VLAN, with PCs connected through the phones.
4. 7 IP cameras (4 external) on a dedicated CCTV VLAN, recording to an on-site NVR.
5. A dedicated management VLAN for network devices.
6. WPA2-secured wireless in each area, including handheld scanners in the warehouse.
7. DHCP for user and voice devices; static addressing for printers, cameras, the NVR and network devices.
8. Inter-VLAN routing for all areas.
9. Internet access through PAT for user and voice VLANs only.
10. SSH-only management, restricted to the Office and Management VLANs.
11. Layer 2 hardening: port security, DHCP snooping, DAI, unused ports disabled, native VLAN changed.

---

## VLAN Design

| VLAN | Name | Subnet | Gateway | Addressing |
|---|---|---|---|---|
| 10 | Office | 192.168.10.0/24 | 192.168.10.1 | DHCP |
| 20 | Counter | 192.168.20.0/24 | 192.168.20.1 | DHCP |
| 30 | Warehouse | 192.168.30.0/24 | 192.168.30.1 | DHCP |
| 40 | Voice | 192.168.40.0/24 | 192.168.40.1 | DHCP |
| 50 | CCTV | 192.168.50.0/24 | 192.168.50.1 | Static |
| 99 | Management | 192.168.99.0/24 | 192.168.99.1 | Static |
| 998 | Unused | – | – | Spare ports, shut down |
| 999 | Native | – | – | Trunk native VLAN only |

---

## Technologies Implemented

- VLANs, 802.1Q trunking and voice VLANs
- Router-on-a-stick inter-VLAN routing
- DHCP server with excluded ranges
- NAT/PAT with an ACL to restrict internet access
- Static default routing
- DNS, HTTP and NTP services
- WPA2-PSK wireless
- IoT CCTV with NVR registration
- Port security (sticky, restrict)
- DHCP snooping and Dynamic ARP Inspection
- PortFast and BPDU Guard
- SSHv2 with local authentication and VTY access control lists

---

## Design Decisions

- **One /24 per VLAN, third octet matches the VLAN ID.** This makes the plan readable at a glance and leaves room to grow. Private address space isn't scarce, so address efficiency wasn't the priority.
- **Router-on-a-stick over a Layer 3 switch.** This suits a small branch. CCTV traffic stays local to the switch (the cameras and NVR share VLAN 50), so the single trunk carries only light inter-VLAN and internet traffic.
- **CCTV and Management have no internet access.** They're excluded from the NAT ACL, which reduces exposure for cameras and network kit.
- **Native VLAN 999 and blackhole VLAN 998.** These prevent VLAN-hopping attacks and stop unused ports being plugged into a live VLAN.
- **Port security maximum of 3 on phone ports.** The phone's MAC is learned in both the voice and data VLANs, plus the PC behind it.
- **Printers trusted for DAI.** In production, an ARP ACL would permit the static printers. The Packet Tracer 2960 doesn't support ARP ACLs, so the printer ports are trusted instead.
- **SW1 takes its time from R1.** R1 syncs NTP from 8.8.8.8 and acts as the local time source for the switch, which has no internet access.

---

## Physical Layer

The design pack documents the physical build to installation standard:

- **Patching schedule:** end-to-end connections with cable IDs (C0001–C0024), VLANs and field-engineer test columns.
- **Rack elevation:** a 15U double-section wall cabinet, with front and rear views and a colour-coded key.
- **Cabling standards:** fibre/copper/power segregation, a 300 mm maximum slack, Velcro every 300 mm, C/M/S label prefixes, and copper certified with a Fluke DSX-5000.
- **Bill of materials:** 24 line items, with indicative pricing.

---

## Key Lessons Learned

- DHCP snooping needs enabling globally as well as per VLAN, otherwise it does nothing.
- Voice VLAN ports need a higher port security maximum than expected.
- Packet Tracer IoT devices default to wireless and need their network adapter changing to wired.
- Verifying after each stage made faults quick to isolate to the part that caused them.