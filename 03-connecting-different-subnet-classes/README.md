# Lab 03 - Connecting Different Subnet Classes (A and B)

## Networking Concept

This lab demonstrates how to connect networks that use **different IP address classes** (Class A and Class B) and route traffic between them using **RIP**. A single router connects three networks spanning two different address classes.

## Topology

```mermaid
graph TD
    PC0["PC0<br/>155.1.0.10/16<br/>Class B"]
    PC2["PC2<br/>10.0.0.10/8<br/>Class A"]
    PC3["PC3<br/>10.0.0.20/8<br/>Class A"]
    PC5["PC5<br/>10.0.0.30/8<br/>Class A"]
    PC6["PC6<br/>155.0.1.10/16<br/>Class B"]

    SW0["Switch0<br/>Cisco IE-2000"]
    SW1["Switch1<br/>Cisco 2960-24TT"]
    SW2["Switch2<br/>Switch-PT-Empty"]

    R0["Router0 (Router-PT)<br/>hostname: Experiment<br/>RIP routing<br/>Fa0/0: 155.1.0.1/16<br/>Fa1/0: 10.0.0.1/8<br/>Fa2/0: 155.0.1.1/16<br/>Serial3/0: clock 2000000"]

    PC0 --- SW0
    PC2 --- SW1
    PC3 --- SW1
    PC5 --- SW1
    PC6 --- SW2

    SW0 ---|Fa0/0| R0
    SW1 ---|Fa1/0| R0
    SW2 ---|Fa2/0| R0
```

## Device Configuration

### Router0 (Router-PT)

| Interface | IP Address       | IP Class | Network           |
|-----------|------------------|----------|-------------------|
| Fa0/0     | 155.1.0.1/16     | B        | 155.1.0.0/16      |
| Fa1/0     | 10.0.0.1/8       | A        | 10.0.0.0/8        |
| Fa2/0     | 155.0.1.1/16     | B        | 155.0.1.0/16      |
| Serial3/0 | -                | -        | clock rate 2000000|

### End Devices

| Device | IP Address       | Subnet Mask      | Gateway      | Class |
|--------|------------------|------------------|--------------|-------|
| PC0    | 155.1.0.10       | 255.255.0.0      | 155.1.0.1    | B     |
| PC2    | 10.0.0.10        | 255.0.0.0        | 10.0.0.1     | A     |
| PC3    | 10.0.0.20        | 255.0.0.0        | 10.0.0.1     | A     |
| PC5    | 10.0.0.30        | 255.0.0.0        | 10.0.0.1     | A     |
| PC6    | 155.0.1.10       | 255.255.0.0      | 155.0.1.1    | B     |

### Switches

- Switch0 (Cisco IE-2000 MultiLayer Switch)
- Switch1 (Cisco 2960-24TT)
- Switch2 (Cisco Switch-PT-Empty)

## Key CLI Commands

### Router0 configuration

```
service password-encryption
hostname Experiment

interface FastEthernet0/0
 ip address 155.1.0.1 255.255.0.0
 no shutdown

interface FastEthernet1/0
 ip address 10.0.0.1 255.0.0.0
 no shutdown

interface FastEthernet2/0
 ip address 155.0.1.1 255.255.0.0
 no shutdown

interface Serial3/0
 clock rate 2000000
 no shutdown

router rip

banner login "hello"
banner motd "This is a secure device"
```

## IP Address Classes Used

| Class | Range                   | Default Mask      | Used in this lab           |
|-------|-------------------------|-------------------|----------------------------|
| A     | 1.0.0.0 - 126.255.255.255 | 255.0.0.0 (/8)  | 10.0.0.0/8 (3 PCs)        |
| B     | 128.0.0.0 - 191.255.255.255 | 255.255.0.0 (/16) | 155.1.0.0/16, 155.0.1.0/16 |

## What This Lab Demonstrates

- **IP address classes** - Class A (10.0.0.0/8) and Class B (155.x.x.x/16) on the same router
- **Inter-class routing** - connecting networks with different class structures via a single router
- **RIP** - distance-vector routing protocol advertising all three networks
- **Serial WAN links** - clock rate configuration on serial interfaces
- **Multiple switches** - connecting end devices across different network segments

## Files

| File                              | Description                          |
|-----------------------------------|--------------------------------------|
| `connecting-subnet-classes.pkt`   | Cisco Packet Tracer lab file (v8.2) |

> Open with Cisco Packet Tracer to view the full topology and device configurations.
