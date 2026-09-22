# Day 04 — Network Fundamentals

**Date:** 2026-09-21

## 🎯 Objective

Build a practical understanding of IP networking and basic network diagnostics.

## 📚 Study

* IP addresses
* Private and public networks
* Subnet masks
* Default gateways
* DNS fundamentals
* TCP and UDP basics

## 📚 What I learned

* An **IP address** identifies a device/interface on a network and allows network communication.
* **Private IP addresses** are used inside local networks, while public IP addresses are used for communication across the Internet.
* A **subnet mask** determines which part of an IP address identifies the network and which part identifies the host.
* The **default gateway** is the router a system uses to reach destinations outside its local network.
* **DNS** translates domain names such as `google.com` into IP addresses.
* `ping` can be used to test basic network reachability.
* `ss` can be used to inspect listening and established network sockets.
* `ip neighbor show` displays information about neighboring devices discovered on the local network.
* `dig` provides detailed DNS lookup information, while `host` provides a simpler DNS lookup.
* A **reverse DNS lookup** can be performed with `dig -x` to query the domain name associated with an IP address.

## 🔨 What I built

Practiced basic Linux network diagnostics using:

```bash
ip addr
ip route
ip neighbor show
```

Tested network connectivity with:

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

Performed DNS lookups with:

```bash
dig google.com
host google.com
dig -x 8.8.8.8
```

Inspected network connections and listening sockets with:

```bash
ss -lntup
ss -tn state established
```

## 🧠 Problems & solutions

### Problem

I initially found it difficult to connect the individual networking commands with the overall network configuration of the system.

### Solution

I grouped the commands by what they help inspect:

* `ip addr` → interfaces and IP addresses
* `ip route` → routing information and default gateway
* `ip neighbor` → neighboring devices on the local network
* `ping` → basic connectivity
* `dig` / `host` → DNS resolution
* `ss` → network sockets and connections

This makes the commands easier to understand as diagnostic tools rather than commands to memorize individually.

## 💡 Key takeaway

Linux provides several small tools that can be combined to understand and troubleshoot network connectivity.

A basic troubleshooting flow is:

```text
Interface / IP
      ↓
Routing / Gateway
      ↓
DNS resolution
      ↓
Connectivity
      ↓
Network connections / sockets
```

I do not need to memorize every networking command yet. The important part is understanding what information each tool provides and when to use it.

## 🔗 Resources

* [Linux Journey — Subnetting Interactive Lessons](https://labex.io/linuxjourney/courses/subnetting)
* [Linux Journey — DNS](https://labex.io/linuxjourney/courses/dns)

## ➡️ Next

Move on to the next milestone and continue building the Linux fundamentals required for the DevOps roadmap.

The detailed documentation of the lab network configuration will be completed once the lab environment has a more complete and stable topology.