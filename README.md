![Topology Diagram](https://github.com/furkangurses/CNNA-Lab-ARP-and-MAC-Address-Learning-in-a-LAN/blob/main/ccna%20lab%20arp.PNG?raw=true)
# CCNA Lab – ARP and MAC Address Learning in a LAN

---

## 🎯 Lab Objective

The purpose of this lab is to understand how communication works inside a **Layer 2 LAN** when:

- MAC address tables are initially empty
- ARP tables are initially empty

This lab demonstrates:

- How ARP resolves an IP address to a MAC address
- How a switch dynamically learns MAC addresses
- The difference between broadcast and unicast traffic
- How ICMP (ping) works after ARP resolution
- How to verify and clear MAC address tables on a switch

> The focus of this lab is understanding Ethernet switching behavior, not memorizing commands.

---

## 🌐 Topology Overview

- 2 Layer 2 switches (**SW1**, **SW2**)  
- 4 PCs (**PC1–PC4**)  
- Network: `192.168.1.0/24`  
- Each switch connects to 2 PCs  
- Switches connected via Gigabit link  

### Initial State

- All switches have **empty MAC address tables**
- All PCs have **empty ARP tables**

---

## 🚦 Traffic Generation

To generate traffic, the following command was used on **PC1**:

```bash
ping 192.168.1.3
```

### Why?

Without traffic:

- Switches cannot learn MAC addresses  
- ARP tables remain empty  

Ping acts as a trigger to initiate:

- ARP resolution  
- ICMP communication  

---

## 🔄 Step-by-Step Packet Flow

---

### 1️⃣ ARP Request (Broadcast)

PC1 wants to ping PC3.

- PC1 knows PC3’s IP address
- PC1 does NOT know PC3’s MAC address

So PC1 sends an ARP request:

- Destination MAC: `FFFF.FFFF.FFFF` (Broadcast)
- Question: *Who has 192.168.1.3?*

#### Switch Behavior

- Learns **PC1’s MAC** from source MAC field  
- Floods broadcast frame out all ports except incoming  

All devices receive it, but only **PC3 replies**.

---

### 2️⃣ ARP Reply (Unicast)

PC3 responds directly to PC1:

- Unicast frame  
- Contains PC3’s MAC address  

#### Switch Behavior

- Learns **PC3’s MAC** from source field  
- Forwards frame only toward PC1  

PC1 updates its ARP table:

```
192.168.1.3 → PC3 MAC
```

---

### 3️⃣ ICMP Echo Request / Reply

After ARP resolution:

- PC1 sends ICMP Echo Request (unicast)  
- Switch forwards based on MAC table  
- PC3 sends ICMP Echo Reply (unicast)  

From this point forward:

✔ Communication is direct  
✔ No more broadcasts required  
✔ Efficient unicast forwarding occurs  

---

## 🧠 Key Switching Logic Demonstrated

This lab proves that a Layer 2 switch:

- Learns MAC addresses from the **source MAC field**
- Does NOT use IP addresses for forwarding
- Floods:
  - Broadcast frames
  - Unknown unicast frames
- Forwards known unicast traffic only to the correct port

---

## 📝 Switch Verification Commands

---

### View MAC Address Table

```bash
show mac address-table
```

#### Purpose

- Verify which MAC address is learned on which interface  
- Confirm dynamic learning  

Displays:

- VLAN  
- MAC address  
- Type (dynamic)  
- Port  

---

### Clear MAC Address Table

```bash
clear mac address-table dynamic
```

#### Purpose

- Remove dynamically learned MAC addresses  
- Force the switch to relearn addresses  

Useful when:

- MAC entries are stale  
- A device changes switch ports  
- Testing learning behavior again  

---

## 💻 PC Verification Command

```bash
arp -a
```

### Purpose

- Verify IP-to-MAC mappings  
- Confirm ARP resolution completed successfully  

---

## 🔎 Key Observations

- First ping may fail due to ARP process  
- ARP request = broadcast  
- ARP reply = unicast  
- Switch learns MAC addresses dynamically  
- After learning, traffic becomes efficient unicast  

---

## 🌍 Real-World Application

This lab directly applies to real troubleshooting scenarios:

- A device cannot reach another device in the same LAN  
- Investigating why ping fails  
- Identifying which device is connected to which switch port  
- Diagnosing MAC learning issues  
- Clearing stale MAC entries  

### In IT Support

- Verify gateway MAC resolution  
- Troubleshoot local LAN communication issues  

### In Junior Network Roles

- Locate devices by MAC address  
- Validate switch learning behavior  
- Perform Layer 2 troubleshooting  

---

## 🛠️ Skills Gained

- Understanding ARP inside a LAN  
- Understanding dynamic MAC learning  
- Differentiating broadcast vs unicast traffic  
- Verifying Layer 2 behavior using CLI  
- Applying Layer 2 logic to troubleshooting  

---

## 🧩 Troubleshooting Notes

If communication fails:

1. Check ARP table:
```bash
arp -a
```

2. Check MAC address table:
```bash
show mac address-table
```

3. Clear MAC table if needed:
```bash
clear mac address-table dynamic
```

Then generate traffic again using:

```bash
ping 192.168.1.3
```

---

## 📚 Conclusion

Understanding Layer 2 behavior is critical for network troubleshooting.

Without mastering:

- ARP
- MAC learning
- Broadcast vs unicast logic

It becomes difficult to diagnose real-world LAN issues effectively.

This lab builds strong foundational knowledge for CCNA-level networking and real IT environments.

---
