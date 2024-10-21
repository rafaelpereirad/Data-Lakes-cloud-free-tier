# Resources used in OCI

![image](https://github.com/user-attachments/assets/4370fd2f-3c2b-4cd3-a3b2-148430add032)


## OCI Region – HA Building Blocks 

• Multiple fault de-correlated, completely independent datacenters: Availability Domain (AD)

• Grouping of hardware and infrastructure within an AD: Fault Domain

• Predictable low latency & high speed, encrypted interconnect between ADs

<img width="660" alt="image" src="https://github.com/user-attachments/assets/932896a9-b68d-4479-bd44-b5e41f48c54a">

Inside an AD – High Scale, High Performance Network

• Non-oversubscribed network; no noisy-neighbors

• Very high scale – ~1 million network ports in an AD

• Predictable low latency & high speed interconnect between hosts in an AD

• Off Box Network Virtualization – moves storage and network IO out of the hypervisor and enables lower overhead and bare metal instances

• Oracle Cloud Infrastructure Services

<img width="660" alt="image" src="https://github.com/user-attachments/assets/9a6cc1e6-75f5-4850-8513-0839851b8226">

## VCN

• A private network that you set up in the Oracle data centers, with firewall rules and specific types of communication gateways that you can choose to use 

• A VCN covers a single, contiguous IPv4 CIDR block of your choice 

• A VCN resides within a single region

• Each VCN network is subdivided into subnets 

• Each subnet can be AD-specific or Regional (recommended) 

• AD specific subnet is contained within a single AD in a multi-AD region 

• Regional subnet spans all three ADs in a multi-AD region 

• Each subnet has a contiguous range of IPs, described in CIDR notation 

## Security Lists

## VPN

## Stacks



