CCNA Lab 02 – ARP and MAC Address Learning in a LAN
📌 Objective

The purpose of this lab is to understand how communication works inside a Layer 2 LAN when MAC address tables and ARP tables are initially empty.

This lab demonstrates:

How ARP resolves an IP address to a MAC address
How a switch dynamically learns MAC addresses
The difference between broadcast and unicast traffic
How ICMP (ping) works after ARP resolution
How to verify and clear MAC address tables on a switch
The focus of this lab is not command memorization, but understanding how Ethernet switching actually behaves.

--------------------------------------------------------------------------------------------------------------

Topology

2 Layer 2 switches (SW1, SW2)
4 PCs (PC1–PC4)
Single LAN network: 192.168.1.0/24
Each switch connects to 2 PCs
Switches connected via a Gigabit link

At the beginning of the lab:

All switches have empty MAC address tables
All PCs have empty ARP tables

--------------------------------------------------------------------------------------------------------------

Traffic Generation

To generate network traffic, the following command was used on PC1: ping 192.168.1.3

Why?

Because without traffic:

Switches cannot learn MAC addresses
ARP tables remain empty
Ping was used as a trigger to initiate ARP resolution and ICMP communication.

--------------------------------------------------------------------------------------------------------------

What Happens Step-by-Step
ARP Request (Broadcast)

PC1 wants to ping PC3.

PC1 knows PC3’s IP address but does NOT know its MAC address.

So PC1 sends an ARP request:

Destination MAC: FFFF.FFFF.FFFF (Broadcast)

Question: “Who has 192.168.1.3?”

Switch behavior:

Learns PC1’s MAC address from the source field
Floods the broadcast frame to all ports except the incoming one
All devices receive it, but only PC3 replies.

--------------------------------------------------------------------------------------------------------------

ARP Reply (Unicast)

PC3 sends an ARP reply directly to PC1:

Unicast frame
Contains PC3’s MAC address

Switch behavior:

Learns PC3’s MAC address from source field
Forwards frame only toward PC1
Now PC1 adds this entry to its ARP table: 192.168.1.3 → PC3 MAC

--------------------------------------------------------------------------------------------------------------

ICMP Echo Request / Reply

After MAC resolution:

PC1 sends ICMP Echo Request (unicast)
Switch forwards based on MAC table
PC3 replies with ICMP Echo Reply (unicast)
From this point forward, communication is direct and efficient.

--------------------------------------------------------------------------------------------------------------

Key Switching Logic Demonstrated

This lab proves that a Layer 2 switch:

Learns MAC addresses from the source MAC field
Does NOT use IP addresses for forwarding

Floods:

Broadcast frames
Unknown unicast frames
Forwards known unicast traffic only to the correct port

--------------------------------------------------------------------------------------------------------------

Switch Verification Commands Used
View MAC Address Table

show mac address-table

Purpose:

Verify which MAC address is learned on which interface
Confirm dynamic learning
This command displays:

VLAN
MAC address
Type (dynamic)
Port

--------------------------------------------------------------------------------------------------------------

Clear MAC Address Table

clear mac address-table dynamic

Purpose:

Remove all dynamically learned MAC addresses
Force the switch to relearn addresses
This is useful in troubleshooting scenarios when:
MAC entries are stale

A device changes ports
Testing learning behavior again

--------------------------------------------------------------------------------------------------------------

PC Verification Command

arp -a

Purpose:

Verify IP-to-MAC mappings
Confirm ARP resolution completed successfully

--------------------------------------------------------------------------------------------------------------

🔎 Key Observations

First ping may fail due to ARP process
ARP request is broadcast
ARP reply is unicast
Switch learns MAC addresses dynamically
After learning, traffic becomes efficient unicast forwarding

--------------------------------------------------------------------------------------------------------------

Real-World Application

This lab directly applies to real troubleshooting situations such as:
“Device cannot reach another device in the same LAN”
Investigating why ping fails
Identifying which device is connected to which switch port
Diagnosing MAC learning issues
Clearing stale MAC entries

In IT Support:

Used to verify gateway MAC resolution
Used when local network communication fails

In Junior Network roles:
Used to locate devices by MAC address
Used to validate switch learning behavior
Used in Layer 2 troubleshooting

--------------------------------------------------------------------------------------------------------------

What I Learned

How ARP enables IP communication inside a LAN
How switches build MAC address tables dynamically
The difference between broadcast and unicast traffic
How to verify Layer 2 behavior using CLI
Why understanding Layer 2 is critical for troubleshooting
