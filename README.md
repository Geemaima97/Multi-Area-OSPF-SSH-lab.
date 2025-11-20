# Multi-Area OSPF + SSH

<img width="649" height="421" alt="ospf topology" src="https://github.com/user-attachments/assets/9a8889c6-9896-4652-95c7-c7685e36ede9" />

## Objective
The goal of this lab is to configure OSPF routing across multiple areas and enable secure remote management using SSH. This lab includes three routers and two PCs, simulating a multi-area network environment with secure administration.

---

## Skills Learned
- OSPF configuration across multiple areas.
- Verifying OSPF adjacency and routing tables.
- Enabling secure SSH access on Cisco routers.
- Router configuration backup and verification.
- Basic troubleshooting of connectivity between PCs across OSPF areas.

---

## Tools Used
- Cisco Packet Tracer (or equivalent network simulator)
- Routers and PCs within the simulation
- Command-line interface for Cisco devices
- Wireshark (optional, for packet verification)

---

## Network Topology
![Network Diagram](ospftopology.png)

---

## Lab Steps

1. **Assign IP Addresses**
   - Assign IP addresses to all router interfaces according to the network diagram.
   - Configure PCs with static IPs in the appropriate subnets for testing.

2. **Configure OSPF**
   - On **R1** (Area 0):
     ```bash
     router ospf 1
     network 10.0.0.0 0.255.255.255 area 0
     ```
   - On **R2** and **R3** (Area 1):
     ```bash
     router ospf 1
     network 10.0.1.0 0.0.0.255 area 1
     network 10.0.0.0 0.255.255.255 area 0
     ```
   - Verify OSPF neighbors using:
     ```bash
     show ip ospf neighbor
     show ip route ospf
     ```

3. **Enable SSH**
   - Configure local username and password:
     ```bash
     username admin privilege 15 secret cisco123
     ```
   - Enable SSH version 2:
     ```bash
     ip domain-name example.com
     crypto key generate rsa
     ip ssh version 2
     line vty 0 4
     login local
     transport input ssh
     ```
   - Verify SSH from PC1:
     ```bash
     ssh -l admin 10.0.0.1
     ```

4. **Verify Connectivity**
   - Ping from PC1 to PC2 to ensure inter-area OSPF routing is working.
   - Check routing tables on all routers to confirm OSPF learned routes.

---

## Files
- **Packet Tracer File:** [01-ospf-multi-area-ssh.pkt](01-ospf-multi-area-ssh.pkt)
- **Router Configs:**
  - [R1_config.txt](configs/R1_config.txt)
  - [R2_config.txt](configs/R2_config.txt)
  - [R3_config.txt](configs/R3_config.txt)
- **Topology Image:** [topology.png](topology.png)
- **Verification Screenshots (optional):**
  - [verify_ospf.png](screenshots/verify_ospf.png)
  - [ssh_test.png](screenshots/ssh_test.png)

---

## Verification
- Ensure all OSPF neighbors are in FULL state:
  ```bash
  show ip ospf neighbor
