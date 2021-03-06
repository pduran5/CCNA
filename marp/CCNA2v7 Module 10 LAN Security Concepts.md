---
marp: true
theme: default
html: true
paginate: true

---
<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

<style scoped>
h1 {
  font-size: 80px;
}
</style>

<!-- _class: invert -->

<!-- _paginate: false -->

# LAN Security Concepts

<!-- _footer: ๐ CCNA2v7 Module 10 LAN Security Concepts\n๐ง๐ปโ๐ซ Pedro Durรกn -->

---

# Networks Attacks
- ๐ฅต **Distributed Denial of Service (DDoS)**
- ๐ **Data Breach**
- ๐ง **Malware** (e.g. WannaCry)

# Network Security Devices
- ๐ **Virtual Private Network (VPN) enabled router**: Can be integrated into firewall.
- ๐ฅ **Next-Generation Firewall (NGFW)**: stateful packet inspection, application visibility and control, Next-Generation IPS (NGIPS), advanced malware protection (AMP), and URL filtering.
- ๐ฎ๐ปโโ๏ธ **Network Access Control (NAC)**: Includes AAA services. Cisco ISE (Identity Services Engine)

---

# Endpoint Protection

๐ Best protected with combination of: NAC + AMP + ESA + WSA
  - **Cisco ESA** (Email Security Appliance): monitors SMTP
  - **Cisco WSA** (Web Security Appliance): advanced malware protection (AMP), application visibility and control, acceptable use policy controls, and reporting.

---

# Access Control - AAA Components
- ๐ฎ๐ปโโ๏ธ **Authentication**: **who is permitted to access** a network
  - **Local Authentication:** SSH in Cisco Routers. Small networks.
  - **Server-Based Authentication:** router accesses (via **RADIUS** or **TACACS+** protocols) a central AAA server that contains usernames and passwords for all users
- ๐ฎ๐ปโโ๏ธ **Authorization**: **what they can do** while they are there. Uses a set of attributes that describe the user's access to the network.
- ๐ฎ๐ปโโ๏ธ **Accounting**: **audit what actions they performed** while accessing the network. Collects and reports usage.

---

# Access Control - 802.1X
 - Port-based access control and authentication protocol.
 - Restricts unauthorized workstations from connecting to a LAN throught publicly accessible switch ports.

 ## 802.1x Roles:
 - **Client (Supplicant)**: Requires access and responds to request from switch
 - **Switch (Authenticator)**: Controls physical access to the network based on client authentication status
 - **Authentication Server**: Performs client authentication.

---

# Layer 2 Vulnerabilities - Switch Attack Categories

๐ **MAC Table Attacks**: MAC address flooding attacks

๐ **VLAN Attacks**: VLAN hopping and VLAN double-tagging attacks

๐ **DHCP Attacks**: DHCP starvation and DHCP spoofing attacks

๐ **ARP Attacks**: ARP spoofing and ARP poisoning attacks

๐ **Address Spoofing Attacks**: MAC address and IP address spoofing attacks

๐ **STP Attacks**: Spanning Tree Protocol manipulation atacks

---

# Layer 2 Vulnerabilities - Switch Attack Mitigation

๐ **Port Security**: Prevents MAC address flooding and DHCP starvation attacks

๐ **DHCP Snooping**: Prevents DHCP starvation and DHCP spoofing attacks

๐ **Dynamic ARP Inspection (DAI)**: Prevents ARP spoofing and ARP poisoning attacks

๐ **IP Source Guard**: Prevents MAC and IP address spoofing attacks

> Also recommended: SSH, SCP, SFTP, SSL/TLS, dedicated management VLAN, ACLs

---

# MAC Address Table Flooding Attack

๐ **Attacker bombards the switch with fake source MAC addresses** โก switch MAC address table full โก **switch floods all incoming traffic out all ports on the same VLAN** โก **attacker captures all of the frames sent from one host to another** on the local LAN or local VLAN.

## Mitigation
๐ **Implement port security.**

![center](img/macfloodingattack.png)

---

# VLAN Hopping Attacks
- ๐ฆ **Enables traffic from one VLAN to be seen by another VLAN without the aid of a router.**
- ๐ฅธ The threat actor **configures a host to act like a switch** to take advantage of the automatic trunking port feature enabled by default on most switch ports โก **establishes a trunk** โก **can access all the VLANs on the switch.**

![center](img/vlanhopping.png)

---

# VLAN Double-Tagging Attacks

- ๐ **Attacker embeds a hidden 802.1Q tag inside the frame that already has an 802.1Q tag:**
  - **1st 802.1 tag โก Native VLAN:** Switch forwards the packet out all native VLAN ports after stripping the VLAN tag โก Attacker must be in a native VLAN port.
  - **2nd inner 802.1 tag** โก The second switch looks only at the inner 802.1Q tag โก frame is destined to the target VLAN.
- ๐ **The attacker gets the ability to communicate with devices on the normally blocked VLAN.**

## Mitigation
๐ **Disable trunking on all access ports**, **disable auto trunking on trunk links** (if needed enabled manually) and be **sure that native VLAN is only used for trunks**.


---

# DHCP Attacks
๐ **DHCP Starvation Attack:** create a DoS for connecting clients. Attacking tools look at the entire scope of leasable IP addresses and try to lease them all.

๐ฅธ **DHCP Spoofing Attack:** a rogue DHCP server is connected to the network and provides false IP configuration parameters to legimitate clients:
  - **Wrong default gateway:** MiTM attack
  - **Wrong DNS server:** points the user to a nefarious website.
  - **Wrong IP address:** DoS attack on the DHCP client.
---

# ARP Attacks

- An attacker can send a gratuitous ARP message containing a spoofed MAC address to a switch, and the switch would update its MAC table accordingly.
- **A threat actor sends unsolicited ARP Replies to other hosts on the subnet with the MAC Address of the threat actor and the IP address of the default gateway**, effectively setting up a **man-in-the-middle attack**.

## Mitigation
๐ **Implement Dynamic ARP Inspection (DAI).**

---

# Address Spoofing Attacks

- ๐ฅธ **IP address spoofing:** when a threat actor hijacks a valid IP address of another device on the subnet or uses a random IP address.
- ๐ฅธ **MAC address spoofing:** when a threat actor alters the MAC address of their host to match another known MAC address of a target host.

## Mitigation
๐ **Implement IP Source Guard (IPSG).**


---

# STP Attack

Network attackers can manipulate the Spanning Tree Protocol to conduct an attack:
- ๐ฅธ **Spoofing the Root Bridge:**
  - **Attacker broadcasts BPDUs with lower bridge priority** to be elected as the **Root Bridge**
  - Changing the topology of the network
- Capturing all traffic for the immediate switched domain

## Mitigation
๐ **Implement BPDU Guard on all access ports.**

---

# CDP Reconnaissance Attack
- **CDP enabled by default** on all Cisco devices.
- CDP information includes the IP address of the device, IOS software version, platform, capabilities and the native VLAN.

## Mitigation
๐ **Limit the use of CDP on devices or ports:**
  - Disable CDP globally on a **device**: `no cdp run`
  - Disable CDP on a **port**: `no cdp enable`