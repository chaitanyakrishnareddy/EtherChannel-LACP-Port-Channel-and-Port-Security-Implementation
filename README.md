EtherChannel (LACP), Port-Channel & Port Security

This project is a focused **Cisco Packet Tracer lab** to practice Layer 2 concepts:
- Bundling links with **EtherChannel (LACP / Port-Channel)**
- Configuring **trunks over the Port-Channel**
- Securing edge ports with **Port Security (sticky MAC, violations)**

---

📌 Topology

- **SW1 ↔ SW2**: Two FastEthernet links combined into a Port-Channel (Po1) using **LACP**  
- **PCs**: Connected as access ports with Port Security enabled  
- **Server**: Optional, can be placed in VLAN 10 (user VLAN) or VLAN 20 (isolated server VLAN)


⚙️ Configurations

VLANs
vlan 10
 name USERS
vlan 20
 name SERVER

Port Security (example for an access port)
interface fa0/1
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
 spanning-tree portfast
 spanning-tree bpduguard enable

EtherChannel with LACP (two links Fa0/23–24)
interface range fa0/23 - 24
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 channel-group 1 mode active   ! use passive on the other switch
!
interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 10,20

🔍 Verification Commands
show etherchannel summary
show lacp neighbor
show interfaces trunk
show interfaces port-channel 1 switchport

show vlan brief

show port-security interface fa0/1
show mac address-table interface fa0/1

🛠 Troubleshooting
Po1 shows (SD), members (s) → Config mismatch (trunking, VLAN list, speed/duplex).
PC ↔ Server ping fails → Check VLAN assignment & allowed VLANs on Po1.
Sticky MAC not learning → Generate traffic or reset with:
default interface fa0/1

🎯 What I Practiced
EtherChannel creation and LACP negotiation
Port-Channel trunk configuration
Port Security (sticky MACs, violation modes, shutdown/recovery)
Troubleshooting using Cisco IOS show commands
