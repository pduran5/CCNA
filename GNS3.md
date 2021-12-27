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

![bg 20% center](https://www.gns3.com/assets/custom/gns3/images/logo-colour.png)
<!-- _footer: GNS3 Recommended Settings\nPedro Durán -->

---

# GNS3 (Graphical Network Simulator-3)
Open-source network software emulator that allows the combination of virtual and real devices, used to simulate complex networks.

![center w:950](img/gns3companies.png)

---

# Classroom Final configuration

Already installed on classroom PCs
- GNS3 version 2.2.21
- VirtualBox

Edit `%APPDATA%\GNS3\2.2\gns3_server.ini` and change `Administrator` to your username (e.g. `alumne`):
```
images_path = C:\Users\alumne\GNS3\images
projects_path = C:\Users\alumne\GNS3\projects
appliances_path = C:\Users\alumne\GNS3\appliances
symbols_path = C:\Users\alumne\GNS3\symbols
configs_path = C:\Users\alumne\GNS3\configs
```

---

# Install GNS3 at home

1. Download `GNS3-2.2.21-all-in-one.exe` from https://github.com/GNS3/gns3-gui/releases/tag/v2.2.21

2. Install selecting VirtualBox.

3. Download and import .ova from `GNS3.VM.VirtualBox.2.2.21.zip`

---

# Emulate PCs ➡️ Alpine Linux dockers

## Installation
- File > New template
- Install an appliance from the GNS3 server (recommended)
- Guests > Alpine Linux
- Install the appliance on the GNS3 VM (recommended)

## IP Configuration

- Before booting up: Right click > Edit config

---

# Emulate Non-Managed Switches ➡️ Ethernet switch

- Run Ethernet switch on **GNS3 VM**

# Emulate Cisco Managed Switches ➡️ Cisco IOU L2

- Download and extract [Switch.zip](https://drive.google.com/file/d/1SNy5iKSojuDbym-KlAOpGhvIZyA7ITzO/view?usp=sharing)
- File > Import appliance
- Appliances > MultiLayer Switch - Cisco IOU L2
- **License:** Edit > Preferences > IOS on UNIX
```
[license]
gns3vm=73635fd3b0a13ad0;
```
:warning: All ports configured as routed ports ➡️ `S1(config-if)# switchport`

---

# Emulate Cisco Managed Routers ➡️ Cisco IOU L3

- Download and extract [Router.zip](https://drive.google.com/file/d/1VFMEIojAZv9bgDQ3b7uIbVDcMNZRnUyw/view?usp=sharing)
- Appliances > Router - Cisco IOU L3
- When imported, right click > Configure template
  - Check `Use default IOU values for memories`. Set RAM size to 512 MB
- **License:** Edit > Preferences > IOS on UNIX
```
[license]
gns3vm=73635fd3b0a13ad0;
```

---

# Emulate OS via VirtualBox

- In VirtualBox, **disconnect all network cards**
- Edit > Preferences > VirtualBox VMs > New
- Run this VirtualBox VM on my local computer
- Choose a VirtualBox VM from the list

# Emulate OS via Docker

- Edit > Preferences > Docker container > New
- Run this Docker container on the GNS3 VM
- New Image: image name from [Docker Hub](https://hub.docker.com/) (e.g. nginx)

---

# Connect to Internet ➡️ NAT

- Fastest
- Run NAT on **GNS3 VM**
- :warning: Cannot access GNS3 topology from external network
- :warning: **Only in A22**: A22 uses 192.168.122.0/24 that conflicts with NAT default assigned range.
  - When loaded GNS3VM, press Enter (OK) > Shell
  - `virsh net-edit default`
  - Change `122` to another number (e.g. `112`)

---

# Connect to Internet ➡️ Cloud with bridged interface

- In GNS3VM, add a **3rd Bridged network card**. Check **Promiscous mode: Allow all**.
- Drag **Cloud**, run on **GNS3VM** and connect to **eth2**
- :warning: **When using an Edge router:**
  - 1. Change MAC address of the interface connected to Cloud (one MAC assigned to each student): `R1(config-if)# mac-address aabb.cc00.0001`
  - 2. NAT overload is needed:
```
R1(config-if)# int e0/0
R1(config-if)# ip nat outside
R1(config-if)# int range e0/1-2
R1(config-if)# ip nat inside
R1(config-if)# exit
R1(config)# ip nat inside source list 1 interface e0/0 overload
R1(config)# access-list 1 permit 172.16.100.0 0.0.0.255
R1(config)# access-list 1 permit 172.16.200.0 0.0.0.255
```