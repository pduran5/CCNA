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

# OSPF

<!-- _footer: CCNA2v7 Module 1 OSPF Concepts\nPedro Durán -->

---

# OSPF (IPv4: OSPFv2 / IPv6:OSPFv3)

- **Link-state** (network prefix, prefix length, cost) routing protocol. Alternative to RIP.
- Faster convergence. Scales to much large network implementations.
- Router builds topology table using **Dijkstra Shortest-Path First (SPF) algorithm** (cumulative cost to reach a destination) to calculate best routes.
- **Areas**: divide the routing domain into areas that help control routing update traffic.
  - **Single-Area OSPF**: all routers in one area. **Area 0**.
  - **Multiarea OSPF**:
    - All areas connected to backbone area (Area 0)
    - Area Border Routers (ABRs): routers interconnecting the areas.
    - Smaller routing tables. Reduced link-state update overhead. Reduced frequency of SPF calculations.

---

# Types of OSPF Packets

5 types of packets (LSP: Link State Packets) to discover neighbors and exchange routing info:

:one: **Hello packet**: Discovers neighbors and builds adjacencies between them
:two: **Database description (DBD)**: Checks for database synchronization between routers
:three: **Link-state request (LSR)**: Requests specific link-state records from router to router
:four: **Link-state update (LSU)**: Sends specifically requested link-state records
:five: **Link-state acknowledgment packet (LSack)**: Acknowledges the other packet types


---

# Components of OSPF - Databases

3 OSPF databases:

:one: **Adjacency Database** (Neighbor Table) `show ip ospf database`
List of all neighbor routers. Unique for each router.
  
:two: **Link-state Database (LSDB)** (Topology Table) `show ip ospf database`
Lists info about all other routers. All routers within an area have same LSDB.

:three: **Forwarding Database** (Routing Table)
List of routers generated when algorithm is run. Unique router's routing table.

---

# Components of OSPF - SPF algorithm
:one: Router builds topology table using Dijkstra shortest-path first (SPF) algorithm (cumulative cost to reach a destination) to calculate best routes.
:two: OSPF places the best routes into the Forwarding Database.

---

# Link-State Operation

Link-state routing steps that are completed by a router:

:one: Establish Neighbor Adjacencies
:two: Exchange Link-State Advertisements
:three: Build the Link State Database
:four: Execute the SPF Algorithm
:five: Choose the Best Route

---
