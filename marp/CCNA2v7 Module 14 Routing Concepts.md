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

# Routing Concepts

<!-- _footer: CCNA2v7 Module 14 Routing Concepts\nPedro Durán -->

---

# Functions of a Router and Longest Match
1. **Determine the best path** to forward packets based on the information in its **routing table**.
2. **Forward packets** towards their destination.

**Best Path** = **Longest Match** =  Routing table entry that has the **greatest number of far-left matching bits with the destination IP address** of the packet.

> Destination IPv4 Address: 172.16.0.10
> Route entries:
> 1. 172.16.0.0/12
> 2. 172.16.0.0/18
> 3. **172.16.0.0/26 ⬅ Longest Match**

---

# Routing Table

- **Directly Connected**: Local interface configured with an IP address and subnet mask and is active (up/up)
- **Remote networks** Networks not directly connected to the router.
  - **Static routes**: route manually configured
  - **Dynamic routing protocols**: routing protocols dynamically learn routes
- **Default route**: next-hop router when routing table does not contain a matching
  - /0 prefix length
  - Also known as **gateway of last resort**

---

# Packet Forwarding Decision Process

![w:950 center](img/packetforwarding.png)

---

# Packet Forwarding Mechanisms
1. **Process switching**
  - Packet arrives on interface ➡ forwarded to control plane ➡ CPU matches destination address in routing table ➡ forwards to exit interface
2. **Fast switching**
  - Uses fast-switching cache to store next-hop information ➡ cache re-used without CPU intervention
3. **Cisco Express Forwarding (CEF)**
  - Default Cisco IOS forwarding mechanism
  - CEF builds a Forwarding Information Base (FIB) and adjacency table.
  - Table entries are change-triggered.

---

# Route Sources

- **`C`**: directly connected network
- **`L`**: address assigned to a router interface (IPv4 /32, IPv6 /128)
- **`S`**: static route
  - small networks not expected to grow significantly
  - path to any network that does not have a more specific match with another route in the routing table
  - routes to and from stub networks.
- **`O`**: dynamically learnt network using OSPF
- **`*`**:  candidate for default route. 0.0.0.0/0 or ::/0

---

# Route Table Entries

```
 O  10.0.4.0/24  [110/50]  via 10.0.3.2,  00:13:29  Serial0/1/0
(1)     (2)       (3)(4)       (5)          (6)         (7)
```

1. Route source
2. Destination network
3. Administrative distance
4. Metric
5. Next-hop
6. Route timestamp
7. Exit interface

When a router has 2 or more paths to a destination with equal cost metrics, then the router forwards the packets using both paths equally. This is called **equal cost load balancing**.

---

# Structure of an IPv4 Routing Table

```
Parent route (Classful network address of this subnet)
   Child route (Indented, Route source and all the forwarding info)
```

Example:
```
Router# show ip route 
(Output omitted) 
   192.168.1.0/24 is variably..
C    192.168.1.0/24 is direct..
L    192.168.1.1/32 is direct..
O    192.168.2.0/24 [110/65]..
O    192.168.3.0/24 [110/65]..
   192.168.13.0/24 is variably..
C    192.168.13.0/30 is direct..
L    192.168.13.1/32 is direct..
   192.168.23.0/30 is subnette..
O    192.168.23.0/30 [110/128]..
```
---

# Administrative distance
```
Route Source         Administrative Distance
-------------------  -----------------------
Directly connected            0 
Static route                  1
EIGRP summary route           5
External BGP                  20
Internal EIGRP                90
OSPF                          110
IS-IS                         115
RIP                           120
External EIGRP                170
Internal BGP                  200
```

---

# Static and dynamic routes
**Static routes:**
- As a **default route forwarding packets to a service provider**
- For **routes outside the routing domain** and not learned dynamically
- When you want to **explicitly define the path** for a specific network
- For routing between **stub networks**

**Dynamic routes:**
- In networks consisting of **more than just a few routers**
- When a **change** in the network topology requires the network to **automatically determine another path**
- For **scalability**. As the network grows, the dynamic routing protocol automatically learns about any new networks.

---

# Dynamic Routing Protocols

```
         Interior Gateway Protocols         Exterior Gateway Protocols

       Distance Vector |   Link-State               Path Vector
       --------------- | --------------             -----------
IPv4    RIPv2  EIGRP   | OSPFv2  IS-IS                 BGP-4
IPv6    RIPng  EIGRP   | OSPFv3  IS-IS                 BGP-MP
```

- **RIP:** Metric is "**hop count**" (each **router** adds a hop). Maximum of **15 hops** allowed.
- **OSPF:** Metric is "**cost**" (based on the **cumulative bandwidth** from source to destination). Faster links are assigned lower costs compared to slower (higher cost) links.
- **EIGRP:** Metric based on the **slowest bandwidth and delay values**. It could also include **load and reliability** into the metric calculation. Supports unequal cost load balancing.