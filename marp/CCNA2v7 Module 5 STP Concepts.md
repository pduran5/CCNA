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

# Spanning Tree Protocol (STP)

<!-- _footer: ð CCNA2v7 Module 5 STP Concepts + CCNA3v6 Chapter 3 STP\nð§ð»âð« Pedro DurÃ¡n -->

---

# Redundancy in Layer 2 Switched Networks
- **Redundancy** â¡ï¸ eliminating **single points of failure**.
- **Path redundancy** â¡ï¸ physical and logical **Layer 2 loops** (frame has not TTL) â¡ï¸ ð¥
- **STP logically blocks physical loops**, preventing frames circling the network forever.
- STP compensates for a failure by recalculating and opening up previously blocked ports.
- **STA: Spanning Tree Algorithm**
  - Creates a loop-free topology by selecting a single root bridge where all the other switches determine a single least-cost path, blocking redundant paths and recalculating in case of Link Failure.

- **â ï¸ STP enabled by default!!!**

---

# STP

- **Problems solved:**
  - ð¥¸ **MAC database instability**: MAC address table constantly changing from the broadcast frames â¡ï¸ High CPU â¡ï¸ Switch unable to forward frames
  - âï¸ **Broadcast Storm**: high number of broadcasts overwhelming the networkð¥
  - ð¯ââï¸ **Duplicated unicast frames:** devices receive twice the same frame from different paths.
- **Usages:**
  - â¾ï¸ **Solves L2 looping problems**
  - ð¤ï¸ **Provides alternative paths in case of failure**
  - âï¸ **Provides VLAN Load Balancing between trunks**

---

# STP Step 1ï¸â£ - Elect the Root Bridge (RB)
- STA designates a single switch as root bridge
- **Root Bridge = Switch with the lowest BID (Bridge ID)**
- **BID  = (Bridge Priority + VLAN ID) . Bridge MAC**
  - **Bridge Priority = 32768** (default). Range: 0...61440 (increments of 4096)
    - **â ï¸ Can be changed to elect another root bridge**
  - **VLAN ID = Extended System ID**
  - **Bridge ID = Switch MAC Address**

> Example:
> S1 BID: `32768.00A0.1101.V001`
> S2 BID: `32768.00A0.FF01.6689`
> S3 BID: `32767.0010.FF32.991B` â¡ï¸ Lowest BID â¡ï¸ Root Bridge

---

# STP Step 1ï¸â£ - Elect the Root Bridge

![w:700 center](img/stpstep1.png)

---

# STP Step 1ï¸â£ - Changing and verify Root Bridge

**Option 1ï¸â£: Select root bridge manually**
```
S1(config)# spanning-tree VLAN 1 root primary
...
S2(config)# spanning-tree VLAN 1 root secondary
```

**Option 2ï¸â£: Change the priority value**
```
S1(config)# spanning-tree VLAN 1 priority 24576
```

**Verify Bridge ID and Root Bridge election**
```
S1# show spanning-tree
```

---

# STP Step 2ï¸â£ - Elect the Root Ports (RP)

- **EVERY NON-ROOT SWITCH will select one Root Port <span style="color:blue">RP</span>.**
  - **Root port** <span style="color:blue">RP</span> (1ï¸â£, if equals then 2ï¸â£, ...):
    - 1ï¸â£ Port with overall lower cost to the Root Bridge
    - 2ï¸â£ Port with lower Sender Bridge ID
    - 3ï¸â£ Port with lower Sender Port Priority
    - 4ï¸â£ Port with lower Sender Port ID

---

# Root Path Cost

## Defaults

![w:500](img/rootpathcost.png)

## Modify Path Cost

```
S1(config)# interface f0/1
S1(config-if)# spanning-tree cost 25
```

---

# STP Step 2ï¸â£ - Elect the Root Ports

![w:700 center](img/stpstep2.png)

---

# STP Step 3ï¸â£ - Elect Designated Ports (DP)

- **EVERY SEGMENT between 2 switches will have one Designated Port <span style="color:green">DP</span>.**
  - All ports of Root Bridge â¡ï¸ <span style="color:green">DP</span>
  - One end of a segment is <span style="color:blue">RP</span> â¡ï¸ Other end is <span style="color:green">DP</span>
  - All ports attached to end devices â¡ï¸ <span style="color:green">DP</span>
  - Other segments without DP, one <span style="color:green">DP</span> (1ï¸â£, if equals then 2ï¸â£, ...):
    - 1ï¸â£ Port with overall lower cost to the Root Bridge
    - 2ï¸â£ Port with lower Sender Bridge ID
    - 3ï¸â£ Port with lower Sender Port Priority
    - 4ï¸â£ Port with lower Sender Port ID

---

# STP Step 3ï¸â£ - Elect Designated Ports

![w:700 center](img/stpstep3.png)

---

# <!--fit--> STP Step 4ï¸â£ - Elect Alternate/Blocked Ports (ALT/BLK)

- **Block ports that are not <span style="color:blue">RP</span> or <span style="color:green">DP</span>** â¡ï¸ **<span style="color:red">ALT/BLK</span>**

![w:700 center](img/stpstep4.png)

---

# Multiple Equal-Cost Path: Lower Sender BID

![w:1000 center](img/lowestsenderbid.png)

> **S2:** <span style="color:blue">RP</span> is F0/1, because S4 has a lower sender BID than S3.
> Same cost â¡ï¸ Different Sender BID

---

# Multiple Equal-Cost Path: Lower Sender Port ID

![w:1100 center](img/lowersenderportpriority.png)

> **S4:** <span style="color:blue">RP</span> is F0/6, because S1 F0/1 has a lower sender Port ID than S1 F0/2.
> Same cost â¡ï¸ Same sender BID â¡ï¸ Same Port Priority â¡ï¸ Different Sender Port ID

---

# STP Timers and Port States
STP convergence requires 3 timers, defined in Root Bridge (changeable):
- ð **Hello Timer**: Interval between PDUS. Default = **2 seconds** (range: 1...10 seconds)
- â¤´ï¸ **Forward Delay Timer**: Time that is spent in the listening and learning state. Default = **15 seconds** (range: 4...30 seconds)
- ðµ **Max Age Timer**: Maximum length of time that a switch waits before attempting to change the STP Topology. Default = **20 seconds** (range: 6...40 seconds)

![w:500 center](img/stpstates.png)

---

# Per-VLAN Spanning Tree (PVST)

**A Root Bridge is elected for EACH spanning tree instance/VLAN â¡ï¸ Load Balancing**
- Cisco switches running IOS 15.0+ run PVST+ by default

![w:700 center](img/pvst.png)

---

# PVST Config: PortFast and BPDU Guard

- **PortFast**: Ports that have end devices (**Access Ports**). Port in FWD state.
- **BPDU Guard**: Disables a PortFast port if a BPDU is received â¡ï¸ Port in `errdisabled`

```
S2(config)# interface range f0/11,f0/18,f0/6
S2(config-if-range)# spanning-tree portfast
S2(config-if-range)# spanning-tree bpduguard enable
```

![w:480 center](img/pvstconfiguration.png)


---

# Different Versions of STP

- **Common STP (CST/STP/802.1D)**: 1998. 1 instance regardless number of VLANs.
- **Per-VLAN STP (PVST+)**: Cisco enhanced STP. 1 instance per VLAN. PortFast, BPDU Guard.
- **Rapid STP (RSTP/802.1w)**: Evolution of STP that provides faster convergence
- **Rapid PVST+**: Cisco enhanced RSTP. 1 instance per VLAN.
- **Multiple STP (MSTP/802.1s)**: Maps multiple VLANs into the same spanning tree instance.
- **Multiple Spanning Tree (MST)**: Cisco enhanced MSTP. Provides up to 16 instances of RSTP.

---

# RSTP
- **Alternate Port** (alternate path to the Root Bridge) â¡ï¸ change to FWD state without waiting the network to converge.
- **Backup Port**: backup to a shared medium (Hub). Less common!!

![w:900 center](img/rstp.png)