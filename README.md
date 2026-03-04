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

