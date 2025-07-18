# Project 9: Creating a VPC Networking Environment for the Cafe

## Overview
This project focuses on creating a secure VPC environment for a web app. It includes configuring a bastion host in a public subnet, launching an EC2 instance in a private subnet, setting up a NAT Gateway, and establishing a secure SSH passthrough between the two instances.

---

## Task 1: Creating a VPC Network for Secure Remote Access

1. **Create a Public Subnet in Lab VPC**
   - Service: VPC → Subnets → Create Subnet
   - VPC ID: Lab VPC
   - Subnet Name: `Public subnet`
   - AZ: Ends with `(a)`
   - IPv4 CIDR Block: `10.0.0.0/24`

2. **Create an Internet Gateway and Attach It to Lab VPC**
   - Service: VPC → Internet Gateways → Create
   - Attach the gateway to Lab VPC

3. **Update Route Table to Route Internet Traffic**
   - Service: VPC → Route Tables → Edit Routes
   - Destination: `0.0.0.0/0`
   - Target: Internet Gateway

---

## Task 2: Creating the Bastion Host (Public EC2)

4. **Launch EC2 Instance**
   - Name: `Bastion Host`
   - AMI: Amazon Linux 2023 (kernel-6.1)
   - Instance Type: `t3.micro`
   - Key Pair: `Vockey`
   - Network: Lab VPC → Subnet: Public subnet
   - Public IP: Enabled
   - SG: `Bastion Host SG`
     - Type: SSH | Port: 22 | Source: My IP

---

## Task 3: Testing the Bastion Host Connection

5. **Convert PPK to PEM (For SSH on Windows)**
   - Download PuTTY + PuTTYgen
   - Load private key → Export OpenSSH key → Save as `labsuser.pem`

6. **Connect to Bastion Host via PowerShell**
```bash
ssh -i "C:\Users\YourName\Downloads\labsuser.pem" ec2-user@<Public-IP>
```

---

## Task 4: Creating a Private Subnet

7. **Create Private Subnet in Lab VPC**
   - CIDR Block: `10.0.1.0/24`
   - AZ: Ends with `(a)`

---

## Task 5: Creating NAT Gateway for Private Subnet

8. **Create NAT Gateway**
   - Subnet: Public Subnet
   - Allocate Elastic IP

9. **Create a Route Table for Private Subnet**
   - Name: `Private Route Table`
   - Attach to Private Subnet

10. **Add Route to NAT Gateway**
   - Destination: `0.0.0.0/0`
   - Target: NAT Gateway

---

## Task 6: Launching EC2 in Private Subnet

11. **Create Key Pair `vockey2`**

12. **Launch Private EC2 Instance**
   - Name: `Private Instance`
   - Subnet: Private Subnet
   - SG: `Private Instance SG`
     - Type: SSH | Port: 22 | Source: `Bastion Host SG`

---

## Task 7: Configure SSH Passthrough

13–16. **SSH Agent Forwarding with PuTTY**
   - Add `vockey2` and `Vockey` in Pageant
   - Open PuTTY → Enable agent forwarding → Load PPK file → Connect

---

## Task 8: Connect from Bastion to Private Instance

17. **Connect to Bastion Host via SSH**

18. **SSH into Private Instance from Bastion**
```bash
ssh ec2-user@<Private-IP>
```

---

## Task 9: Enhancing Security with Network ACL

19. **Create Network ACL for Private Subnet**
   - Associate it with Private Subnet
   - Configure inbound/outbound rules to allow traffic

---

## ✅ Project Completed

This completes the secure VPC setup with a bastion host and a private subnet, along with enhanced network ACL security.