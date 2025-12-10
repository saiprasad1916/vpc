# AWS Asymmetric VPC Architecture — Full Deployment Guide  
Production Grade — Variable Size Subnets — Manual AWS Console Deployment

---

## 🚀 Project Overview

This documentation guides the deployment of a **production-grade AWS VPC** that contains **variable size subnets** to simulate a real-world scalable network environment with optimized IP allocation.

The goal is to design a network that supports **public-facing infrastructure**, **private backend workloads**, and **secure isolated services**.

---

## 🎯 Objective

Build a VPC architecture with:

- Asymmetric subnet sizes
- Public and private segmentation
- Secure private routing
- Scalable subnet CIDRs
- Proper tagging for billing and governance

---

## 🧱 VPC Configuration

| Field | Value |
|------|------|
| VPC Name | asymmetric-vpc |
| CIDR | `10.0.0.0/16` |
| DNS Hostnames | Enabled |
| DNS Resolution | Enabled |

### Required Global Tags

| Key | Value |
|------|------|
| Environment | production |
| Owner | network-team |
| Project | asymmetric-vpc-build |
| CostCenter | AWS-Networking |

---

## 🌐 CIDR Block Allocation

| Subnet Name | Type | CIDR | Capacity |
|------------|------|------|----------|
| Public Subnet A | Public | `10.0.0.0/24` | ~256 IPs |
| Public Subnet B | Public | `10.0.16.0/20` | ~4096 IPs |
| Public Subnet C | Public | `10.0.32.0/19` | ~8192 IPs |
| Private Subnet A | Private | `10.0.64.0/22` | ~1024 IPs |
| Private Subnet B | Private | `10.0.68.0/23` | ~512 IPs |
| Private Subnet C | Private | `10.0.96.0/19` | ~8192 IPs |

All CIDRs are **non-overlapping** within VPC range `10.0.0.0/16`.

---

## 📡 Internet Gateway Configuration

| Action | Name | Attached To |
|--------|------|-------------|
| Create Internet Gateway | `asymmetric-igw` | asymmetric-vpc |

---

## 🗺 Route Table Design

| Route Table | Associated Subnets | Route |
|------------|--------------------|-------|
| Public-RT | Public A, B, C | `0.0.0.0/0 → IGW` |
| Private-RT-A | Private Subnet A | Local Only |
| Private-RT-B | Private Subnet B | Local Only |
| Private-RT-C | Private Subnet C | Local Only |

---

## 📍 Subnet – Route Table Association

| Subnet | Route Table |
|--------|------------|
| Public Subnet A | Public-RT |
| Public Subnet B | Public-RT |
| Public Subnet C | Public-RT |
| Private Subnet A | Private-RT-A |
| Private Subnet B | Private-RT-B |
| Private Subnet C | Private-RT-C |

---

## 🔁 Route Entries Summary

### Public Route Table
| Destination | Target |
|------------|--------|
| `0.0.0.0/0` | Internet Gateway |

### Private Route Tables
| Destination | Target |
|------------|--------|
| None | Local only |

---

## 📊 Final Architecture Diagram (ASCII)

