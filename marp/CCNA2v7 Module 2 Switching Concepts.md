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

# Switching Concepts

<!-- _footer: ð CCNA2v7 Module 2 Switching Concepts\nð§ð»âð« Pedro DurÃ¡n -->

---
<!-- _header: "Frame Forwarding" -->

![bg auto right:46%](img/switchforwarding.png)

# Switching in Networking

## Ports Types:
- â¤µï¸ **Ingress**: entering the interface
- â¤´ï¸ **Egress**: exiting the interface

## Forwarding frames:
  - â¤µï¸ **Ingress Interface**
  - **Destination MAC address** â¡ï¸ Egress
  - **Using its MAC Address Table** â¡ï¸ Ingress Source MAC Adress

---

<!-- _header: "Frame Forwarding" -->

# The Switch Learn and Forward Method

1ï¸â£ **Learn â Examines Source Address**
   - âð» Adds the source MAC if not in table
   - ð Resets the time out setting back to 5 minutes if source is in the table

2ï¸â£ **Forward â Examines Destination Address**
   - ð¤ Destination MAC in MAC address table? â¡ï¸ forward out the specified port
   - ð¤ Destination MAC is not in the table? â¡ï¸ flooded out all interfaces except the one it was received.

---

<!-- _header: "Switch Forwarding Methods" -->
# Switch Forwarding Methods: Store-and-Forward
- **Error checking**: Check FCS for CRC errors. Bad frames discarded
- **Buffering**: Buffer frame while it checks FCS.

![center](img/storeandforward.png)

---

<!-- _header: "Switch Forwarding Methods" -->
# Switch Forwarding Methods: Cut-Through
- âï¸ **Cut-through**: Forwards frame after Destination MAC.
- **Fragment Free**: At least 64 bytes. Eliminates runts.
- **â ï¸ Does not check FCS â¡ï¸ It can propagate errors**

![center](img/cutthrough.png)

---

# Broadcast domain
- Router: ð´
- Switch: ð¢ 
- Hub: ð¢

# Collision domain
- Router: ð´
- Switch: ð´
- Hub: ð¢

![bg auto right:58%](img/collisionbroadcastdomain.png)

---

# Alleviated Network Congestion

- MAC Address Table
- Full-duplex
- Fast Port Speeds
- Fast Internal Switching
- Large Frame Buffers
- High Port Density