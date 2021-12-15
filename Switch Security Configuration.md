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

# Switch Security Configuration

<!-- _footer: CCNA2v7 Module 11\nPedro Durán -->

---

# Implement Port Security

- ⚠ **Secure all unused ports on a switch**
```
S1(config)# interface range f0/10-f0/24
S1(config-if)# shutdown
```

- ⚠ **Enable port security**
  - Limits the number of valid MAC addresses allowed on a port):
    - Manually configured
    - Dynamically learnt
```
S1(config)# interface f0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
S1(config-if)# do show port-security interface f0/1
```
---

# Limit and Learn MAC Addresses

Set the maximum number of MAC addresses allowed on a port (default: 1):
```
S1(config-if)# switchport port-security maximum 4
```

**1. Manually Configured**
```
S1(config-if)# switchport port-security mac-address aaaa.bbbb.1234
```

**2. Dynamically Learnt:**
- Current source MAC for the device connected to the port is secured
  - ⚠ NOT added to `running-config`!!!
```
S1(config-if)# switchport port-security
```
---

# Limit and Learn MAC Addresses

**3. Dynamically Learned - Sticky**
- Dynamically learns the MAC address and stick them to `running-config` ➡ `wr`

```
S1(config-if)# switchport port-security mac-address sticky
```

**Example:**
```
S1(config)# interface f0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security maximum 4
S1(config-if)# switchport port-security mac-address aaaa.bbbb.1234
S1(config-if)# switchport port-security mac-address sticky
S1(config-if)# exit
S1# show port-security interface f0/1
S1# show port-security address
```

---

# Port Aging Security

Can be used to set the aging time for static and dynamic secure addresses on a port:
- **Absolute**: deleted after the specified aging time.
```
S1(config)# interface f0/1
S1(config-if)# switchport port-security again time 10
S1(config-if)# switchport port-security again type absolute
```
- **Inactivity**: deleted if they are inactive for a specified time.
```
S1(config)# interface f0/1
S1(config-if)# switchport port-security again time 10
S1(config-if)# switchport port-security again type inactivity
```

---

# Port Security Violation Modes

If a **MAC address** of a device attached to a port **differs from the list of secure addresses** ➡ **port violation** occurs ➡ port enters **error-disabled state**. 3 modes:

- **shutdown** (default): 💥 **error-disabled** inmediately + 🔴 **turns off port LED** + 📩sends **syslog** message + 💦 **increments violation counter**.
**Reenable:** `shutdown` + `no shutdown`

- **restrict**: port drops packets with unknown source addresses until you remove a sufficient number of secure MAC addresses to drop below the max value or increase the max value. 💥 **error-disabled** + 📩 send **syslog** message + 💦 **increments violation counter**

- **protect**: port drops packets with unknown source addresses until you remove a sufficient number of secure MAC addresses to drop below the max value or increase the max value. 💥 **error-disabled** + ⚠ **no syslog message**

---

# Port Security Violation: shutdown mode
```
S1# show interface f0/18
FastEthernet0/18 is down, line protocol is down (err-disabled)

S1# show port-security interface f0/18
Port Security: Enabled
Port Status: Secure-shutdown
Violation Mode: Shutdown
Security Violation Count: 1
...

S1# configure terminal
S1(config)# interface f0/18
S1(config-if)# shutdown
S1(config-if)# noshutdown
```

```
S1(config)# interface f0/18
S1(config-if)# switchport port-security violation restrict
...
```
