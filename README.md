# Hybrid Network Design

## Overview

This repository documents the design of a hybrid enterprise network architecture that integrates on-premises infrastructure with cloud networking.

The architecture includes identity services, segmented internal networks, application servers, database systems, and centralized storage. A firewall provides routing, segmentation, and secure connectivity between internal systems and cloud environments.


## Network Segmentation

The internal network is divided into multiple VLANs to isolate different infrastructure components and improve security.

| VLAN | Network | Purpose |
|-----|------|------|
| 10 | 192.168.10.0/24 | Management |
| 20 | 192.168.20.0/24 | Servers |
| 30 | 192.168.30.0/24 | Security |
| 40 | 192.168.40.0/24 | Clients |

Each VLAN uses the firewall as the default gateway. This segmentation model reduces unnecessary communication between systems and helps enforce security boundaries.

## Infrastructure Components

The hybrid network environment includes several core infrastructure services that support identity management, application hosting, data storage, and client access.

### Identity Infrastructure

Active Directory provides centralized authentication and directory services across the network.

Capabilities include:

- User authentication
- Group policy management
- Centralized authorization
- DNS services

These services allow systems and users to authenticate and access resources securely.

---

### Client Systems

Windows clients join the domain to simulate enterprise endpoint environments.

Domain-joined clients receive:

- Domain authentication
- Group policy configuration
- Controlled access to shared resources

This enables centralized management of user systems.

---

### Application Infrastructure

Applications follow a multi-tier architecture separating client systems, application servers, and database services.

Client  
│  
Web Server  
│  
Database Server  

This design separates presentation, application logic, and data storage layers.

---

### Database Infrastructure

The database server stores and manages application data.

Access to the database is restricted to application servers to reduce exposure and enforce security boundaries.

---

### Storage Infrastructure

A centralized storage server provides shared file services for users and servers.

Example storage structure:

/srv/storage  
├── shared  
├── backups  
└── projects  

Centralized storage simplifies data management and supports backup strategies.


## Hybrid Connectivity

Hybrid connectivity allows the on-premises infrastructure to communicate securely with cloud environments.

This design uses a **site-to-site IPsec VPN** between the on-prem firewall and a cloud VPN gateway.

Cloud Network  
10.x.x.x  
│  
Cloud VPN Gateway  
│  
IPsec Tunnel  
│  
Firewall  
│  
On-Prem Network  
192.168.x.x  

The firewall acts as the VPN gateway and routes traffic between internal networks and the cloud virtual network.

This configuration allows:

- Secure communication between on-prem servers and cloud workloads
- Extension of internal services into cloud environments
- Controlled routing between network segments and cloud infrastructure

## Design Principles

The hybrid network architecture follows several core design principles commonly used in enterprise environments.

**Network Segmentation**

Infrastructure components are separated into different VLANs to reduce unnecessary communication between systems and limit potential lateral movement.

**Centralized Identity Management**

Active Directory provides centralized authentication and authorization for users, servers, and client systems.

**Layered Architecture**

Application services follow a multi-tier architecture separating client systems, application servers, and database services.

**Secure Hybrid Connectivity**

A site-to-site IPsec VPN allows secure communication between the on-prem network and cloud environments.

**Controlled Network Access**

Firewall policies regulate communication between network segments and external environments.

                    Cloud Environment
                  (Azure / AWS VNet)
                         │
                         │
                  +---------------+
                  |   VPN Gateway |
                  +---------------+
                         │
                     IPsec VPN
                         │
                         │
                  +---------------+
                  |    pfSense    |
                  | Firewall/VPN  |
                  +---------------+
                         │
          ───────────────┼────────────────
                         │
                Network Segmentation
                         │

        ┌───────────────┬───────────────┬───────────────┬───────────────┐
        │               │               │               │               │
     VLAN 10         VLAN 20         VLAN 30         VLAN 40
   Management         Servers        Security         Clients
  192.168.10.0     192.168.20.0    192.168.30.0     192.168.40.0

        │               │               │               │
        │               │               │               │
   +-----------+   +-----------+   +-----------+   +-----------+
   |  Domain   |   |   Web     |   |  Security |   |  Windows  |
   |Controller |   |  Server   |   |  Tools    |   |  Clients  |
   |  AD/DNS   |   |           |   | (Kali etc)|   |           |
   +-----------+   +-----------+   +-----------+   +-----------+
                         │
                         │
                   +-----------+
                   | Database  |
                   |  Server   |
                   +-----------+
                         │
                   +-----------+
                   |  Storage  |
                   |  Server   |
                   +-----------+
