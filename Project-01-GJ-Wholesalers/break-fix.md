# Break / Fix – Project 1: GJ Wholesalers Ltd

Real faults found during the build, how each was diagnosed, and why the network behaved the way it did.


## Fault 1 – R1 could not reach the simulated internet

Symptom:
The ISP router could ping 8.8.8.8, but R1 could not.

Diagnosis:
R1 could ping its next hop (203.0.113.2), so the WAN link was fine and the fault was beyond the ISP. Checked GOOGLE-DNS's IP configuration.

Root cause:
GOOGLE-DNS had no default gateway configured.

Fix:
Set GOOGLE-DNS's default gateway to 8.8.8.1.

Why:
The ISP shares a subnet with the server, so it needs no gateway. R1's pings came from 203.0.113.1, an off-subnet address. The server received them but had no route to send the replies back.


## Fault 2 – DHCP snooping binding table empty

Symptom:
show ip dhcp snooping binding showed 0 bindings, even though PCs were receiving DHCP addresses.

Diagnosis:
Checked the top of show ip dhcp snooping. It showed snooping was disabled, despite VLANs 10–40 being configured.

Root cause:
Snooping was enabled per VLAN, but not globally.

Fix:
Entered ip dhcp snooping in global configuration, then renewed DHCP on the clients.

Why:
DHCP snooping needs both the global command and the per-VLAN command. Without the global one, the switch doesn't inspect DHCP traffic, so no bindings are built. This would have broken DAI later.


## Fault 3 – Port security violations on IP phone ports

Symptom:
Repeated PORT_SECURITY-2-PSECURE_VIOLATION messages on F0/1, F0/2, F0/5 and F0/8, and the phones were missing from the DHCP snooping bindings.

Diagnosis:
Violations appeared only on the phone ports, which were set to maximum 2 (phone + PC).

Root cause:
On a port with a voice VLAN, the phone's MAC is learned in both the voice VLAN and the data VLAN, plus the PC's MAC, so 3 addresses were needed.

Fix:
Raised the port security maximum to 3 on the phone ports, then power-cycled the phones.

Why:
In restrict mode, the third MAC's traffic was dropped, which blocked the phones' DHCP requests. After the fix there were 14 bindings and 0 violations.


## Fault 4 – ARP ACL not supported for static printers

Symptom:
The arp access-list command was unrecognised, and ip arp inspection offered no filter option.

Diagnosis:
Checked the available DAI options with ?. Only validate and vlan exist on the Packet Tracer 2960.

Root cause:
A Packet Tracer limitation. ARP ACLs aren't implemented.

Fix:
Trusted the printer ports (F0/3, F0/6, F0/9) for DAI instead.

Why:
The printers are statically addressed, so they have no DHCP snooping binding, and DAI would drop their ARP traffic. In production, an ARP ACL permitting each printer's IP and MAC is the more secure option, because trusted ports aren't inspected at all.


## Summary

1. Server missing default gateway – Layer 3 / Addressing
2. DHCP snooping not enabled globally – Layer 2 security
3. Port security max too low on voice ports – Layer 2 security
4. ARP ACL unsupported – Platform limitation