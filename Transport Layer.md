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

# ICMP

<!-- _footer: "Pedro Durán" -->

---

# ICMP Messages

Messaging protocol for IPv4 (ICMPv4) and IPv6 (ICMPv6, includes additional functionality).

- **Host reachability:**
ICMP Echo Message can be used to test the reachability of a host on an IP network.
- **Destination or Service Unreachable:**
ICMP Destination Unreachable message can be used to notify the source that a destination or service is unreachable. 
- **Time exceeded:**
When the Time to Live field (IPv4 TTL field, IPv6 Hop Limit field) in a packet is decremented to 0, an ICMP Time Exceeded message will be sent to the source host. 

---

# ICMPv6 Messages

Neighbor Discovery Protocol (NDP) messages:

- **Router Solicitation (RS) and Router Advertisement (RA):**
  - Messaging between an IPv6 router and an IPv6 device
  - Dynamic address allocation.
- **Neighbor Solicitation (NS) and Neighbor Advertisement (NA):**
  - Messaging between IPv6 devices
  - Duplicate address detection (DAD)
  - Address resolution

---

# ICMPv6 RS Messages
- **Sent by hosts**
- To determine how to receive its IPv6 address information dynamically. 

# ICMPv6 RA Messages

- **Sent by routers:**
  - Every 200 seconds
  - In response to an RS message.
- **Provide addressing information to IPv6-enabled hosts:**
  - Prefix, prefix length, DNS address, and domain name.
  - Default gateway = link-local address of the router that sent the RA.

---

# ICMPv6 NS Messages

- **Sent by hosts**
- To perfom **DAD (Duplicate Address Detection)**:
  - Check the uniqueness of an address
  - Host sends its own GUA/LLA IPv6 address as the targeted IPv6 address.
  - If another device has this address ➡️ Sends NA message.
- **To determine the MAC address of the destination:**
  - Device sends NS message to solicited node address.
  - Targeted device ➡️ Responds NA message with MAC address.

---

# Test connectivity - Ping

  - Uses ICMP Echo and Reply messages
  - **Common for the 1st ping to timeout** if address resolution (ARP or ND) needs to be performed before sending the ICMP Echo Request.
  - 🛠️ **Steps to network troubleshooting:**
    - 1️⃣ Ping the Loopback (127.0.0.1 or ::1/64)
    - 2️⃣ Ping the Default Gateway
    - 3️⃣ Ping the Remote Host

---

# Test the path - Traceroute 

- Test the path between two hosts. Command: `traceroute` or `tracert`
- Provide a list of hops that were successfully reached along that path.
  - 🟢 Round-trip time for each hop
  - 🔴 * (lost or unreplied packet)
- Inner working:
  - 1st message:   TTL=1 ➡️ 1st router responds with Time Exceeded message
  - 2nd message: TTL=2 ➡️ 2nd router responds with Time Exceeded message
  - ...