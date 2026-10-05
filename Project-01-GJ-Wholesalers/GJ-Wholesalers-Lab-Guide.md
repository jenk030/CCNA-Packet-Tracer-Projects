# Project 1 – GJ Wholesalers Ltd: Branch Network Lab

## Scenario 

GJ Wholesalers Ltd is an electrical wholesaler opening a new trade branch. The site has a back office, a customer-facing trade counter and a stock warehouse. The branch runs independently of head office and needs a secure, segmented network with voice, wireless, CCTV and internet access, all delivered from a single wall-mounted comms cabinet.

This lab is the build plan. Each part lists what to configure not the commands. All addressing, VLANs and port assignments are in the project spreadsheet (IP Allocation and Patching Schedule tabs).

---

## Part 1 – Initial Setup 

1. Configure the appropriate hostname on R1, SW1 and ISP. 
2. Disable DNS lookup of mistyped commands on R1 and SW1. 
3. Configure a login banner on R1 and SW1 warning that access is restricted to authorised users. 

> Passwords, console login and SSH are deliberately left until Part 9, so the build isn't slowed down by logging in at every step.

## Part 2 – VLANs and Switch Ports (SW1)

1. Create and name all VLANs from the IP Allocation tab, including UNUSED (998) and NATIVE (999). 
2. Configure the access ports according to the Patching Schedule:
   a. Manually set each port to access mode and explicitly disable DTP. 
   b. Assign the correct data VLAN to each port. 
   c. On IP phone ports, also assign voice VLAN 40.
3. Configure G0/1 as a trunk to R1:
   a. Manually set trunk mode and explicitly disable DTP. 
   b. Set the native VLAN to 999. 
   c. Allow only VLANs 10, 20, 30, 40, 50 and 99. 
4. Assign all spare ports to VLAN 998 and administratively disable them. 
5. Enable PortFast and BPDU Guard on every port connected to an end device (phones, printers, APs, cameras, NVR).

> **Checkpoint:** 
1. Every access port in the correct VLAN and G0/1 absent from the list. 
2. Shows G0/1 trunking with native VLAN 999 and allowed VLANs 10, 20, 30, 40, 50, 99. 

## Part 3 – IP Addressing and Inter-VLAN Routing

1. On R1, enable G0/1 and configure router-on-a-stick:
   a. Create one subinterface per VLAN (10, 20, 30, 40, 50, 99), with the subinterface number matching the VLAN ID. 
   b. Configure 802.1Q encapsulation and the gateway IP on each. 
   c. Add a description to each subinterface.
2. On R1, configure G0/0 with its WAN address and enable it. 
3. On ISP, configure G0/0 and G0/1 and enable them. 
4. On SW1, configure the VLAN 99 SVI and a default gateway so it can be managed from other VLANs. 
5. Manually configure the IP settings (address, mask, gateway) of GOOGLE-DNS, NVR1, the printers and all cameras.

> **Checkpoint:** 
1. On R1 shows G0/0 and all six subinterfaces up/up. 
2. From SW1, ping each VLAN gateway. 

## Part 4 – Routing

1. Configure a static default route on R1 pointing to the ISP's next-hop address.  
2. Do **not** configure any route on ISP back to the 192.168.x.x networks. 
3. Verify R1 can reach 8.8.8.8.

> **Checkpoint:**  
1. On R1 shows the default route. 
2. R1 can ping 8.8.8.8. 

## Part 5 – Network Services

1. **DHCP** – configure R1 as the DHCP server:
   a. Exclude .1 – .10 in VLANs 10, 20, 30 and 40. 
   b. Create pools named OFFICE, COUNTER, WAREHOUSE and VOICE. 
   c. Each pool must hand out its subnet, default gateway and DNS server 8.8.8.8. 
   d. No DHCP pools for CCTV or MGMT. 
2. **DNS** – on GOOGLE-DNS, enable the DNS service and create an A record for **www.google.com** → 8.8.8.8. Make sure HTTP is enabled.
3. **NAT/PAT** – configure PAT on R1 so the user and voice VLANs can reach the internet:
   a. Use standard ACL 1 to match VLANs 10, 20, 30 and 40 only. CCTV and MGMT must **not** have internet access. 
   b. Mark the inside and outside interfaces correctly. 
   c. Overload the translation onto G0/0. 
4. **NTP** – keep all devices on the same time so log timestamps line up:
   a. Enable the NTP service on GOOGLE-DNS.
   b. Configure R1 to learn the time from 8.8.8.8. 
   c. Configure SW1 to learn the time from R1. SW1 has no internet access, so R1 acts as the local time source. If SW1 will not sync, configure R1 to act as an NTP master. 
5. R1 and SW1 shows a synchronised server (`*`). If SW1 will not sync, configure R1 as an NTP master.

> **Checkpoint:** 
1. PCs and phones receive addresses from the correct pool. 
2. A PC can ping 8.8.8.8 and browse to www.google.com. 
3. Shows PAT entries. A camera or SW1 cannot reach 8.8.8.8 

## Part 6 – Wireless

1. Configure each AP with its own SSID and WPA2-PSK: 

| AP | SSID | Passphrase |
|---|---|---|
| AP1 | GJW-OFFICE | GJWoffice1 |
| AP2 | GJW-COUNTER | GJWcounter1 |
| AP3 | GJW-WAREHOUSE | GJWwarehouse1 |

2. Connect each wireless client to the correct SSID for its area. 
3. Verify each client receives an address from its area's DHCP pool. 

> **Checkpoint:** 
1. Every wireless client has an IP from its own area's DHCP pool and can reach 8.8.8.8. 

## Part 7 – CCTV

1. **Prepare each camera:**
   a. In Advanced → I/O Config, set Network Adapter to the wired adapter (PT-IOT-NM-1CGE) and Network Adapter 2 to None, so the camera has no wireless interface. 
   b. In Config, set the static IP on the wired interface and set the gateway to Static – 192.168.50.1. 
   c. Make sure the camera is powered on (icon dark, not greyed out). 
2. **Enable the IoT server** – on NVR1, go to Services → IoT and set it to On. 
3. **Create the NVR account** – from PC1, open Desktop → Web Browser, browse to 192.168.50.11 and choose Sign up now.
   a. Username **cctvadmin** password **gjwcctv1** 
4. **Register the cameras** – on each camera, go to Config → Settings → IoT Server, select Remote Server and enter:
   a. Server Address 192.168.50.11, username cctvadmin, password gjwcctv1. 
   b. Click Connect once only – the button changes to Refresh when registered. Clicking Connect again creates duplicate entries. 
5. **Verify** – from PC1, log in to http://192.168.50.11 and check the device list. 

> **Checkpoint:** 
1. Exactly seven devices are listed: EXT-1, EXT-2, EXT-3, EXT-4, OFF-1, COU-1 and WAR-1. 
2. Every device shows a green status (red = offline or stale registration). 
3. No duplicate entries. If there are, turn the NVR1 IoT service off and on, or create a new account and reconnect each camera once. 
4. From NVR1's Command Prompt, each camera (192.168.50.12 – .18) replies to a ping.

## Part 8 – Layer-2 Security Features

1. **Port Security** – configure on all active access ports on SW1 (set maximum, violation and sticky first, enable last):
   a. IP phone ports: allow a maximum of 3 MAC addresses (the phone is learned in both the voice and data VLANs, plus the PC behind it). 
   b. AP ports: allow enough MAC addresses for the wireless clients behind each AP (5). 
   c. All other end-device ports: allow a maximum of 1 MAC address. 
   d. Use a violation mode that drops invalid traffic without shutting the port and sends a notification (restrict). 
   e. Automatically save learned secure MAC addresses to the running-config (sticky). 
2. **DHCP Snooping** – configure on SW1:
   a. Enable DHCP snooping globally. 
   b. Enable it for VLANs 10, 20, 30 and 40. 
   c. Trust the appropriate port (G0/1 – towards the DHCP server on R1). 
   d. Disable insertion of DHCP Option 82. 
   e. Set a DHCP rate limit of 15 pps on active untrusted access ports. 
   f. Renew DHCP on every DHCP client (PCs, phones, wireless clients) so they all appear in the binding table before DAI is enabled. 
3. **Dynamic ARP Inspection** – configure on SW1:
   a. Enable it for VLANs 10, 20, 30 and 40. 
   b. Trust the appropriate port (G0/1). 
   c. Make sure the statically addressed printers can still communicate. In production this would be done with an ARP ACL; Packet Tracer's 2960 does not support ARP ACLs, so the printer ports (F0/3, F0/6, F0/9) are trusted instead. 
4. After enabling each feature, re-test DHCP, inter-VLAN connectivity, internet access and printing before moving on. 

> **Checkpoint:**
1. All 18 ports listed with the correct maximum, Restrict action and 0 violations. 
2. 14 bindings (4 PCs, 4 phones, 6 wireless clients). 
3. G0/1, F0/3, F0/6 and F0/9 trusted, all others untrusted.
4. After `arp -d`, one PC per VLAN (PC1, PC3, PC4) can ping its printer, its gateway and 8.8.8.8. PC1 
5. Phones still hold a 192.168.40.x address.

## Part 9 – Device Access and Passwords

Complete this part last, once everything else is built and tested.

1. Configure the enable secret **gjwenable** on R1 and SW1.
2. Configure the local user account **netadmin** with secret **gjw123** on R1 and SW1. 
3. Configure the console line on R1 and SW1:
   a. Require login with a local user account. 
   b. Set a 15-minute inactivity timeout. 
   c. Enable synchronous logging. 
4. **SSH** – configure secure remote access on R1 and SW1:
   a. Set the domain name **gjwholesalers.local**. 
   b. Generate RSA keys of at least 2048 bits. 
   c. Allow SSHv2 only. 
   d. Allow only SSH on all VTY lines (0–15), with local user login and synchronous logging. 
5. **SSH access control** – restrict who can manage the network:
   a. Create standard ACL 10 permitting only the MGMT (VLAN 99) and Office (VLAN 10) subnets. 
   b. Apply it to the VTY lines on R1 and SW1. 
   c. Verify SSH from a Counter or Warehouse device is refused. 
6. Encrypt all plaintext passwords in the configuration of R1 and SW1. 
7. Save the running configuration on R1 and SW1. 

> **Checkpoint:**
1. SSH to R1 and SW1 works from an Office PC (PC1).
2. SSH is refused from a Counter or Warehouse PC (PC3 / PC4).
3. Telnet to R1 fails from PC1.
4. Console login prompts for the local user netadmin.

