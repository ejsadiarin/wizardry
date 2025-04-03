---
tags:
  - How-to
date: 2024-08-11T13:09
---

<!--toc:start-->

- [Networking (CCNA, etc.)](#networking-ccna-etc)
- [Subnetting](#subnetting)
  - [Learn the subnetting trick](#learn-the-subnetting-trick)
    - [Example Problems](#example-problems)
      - [Construct Subnet Table](#construct-subnet-table)
      - [Use given CIDR/mask to find column on the table](#use-given-cidrmask-to-find-column-on-the-table)
      - [Get relevant octet range given the CIDR/Subnet](#get-relevant-octet-range-given-the-cidrsubnet)
  - [Speed Tips](#speed-tips)
- [IEEE Standards for 802.1xx](#ieee-standards-for-8021xx)
<!--toc:end-->

<!-- 2024-08-11 (August 11, 2024 1:09 PM Sunday) -->

# Networking (CCNA, etc.)

learning plan:

- Subnetting
- Recursive static routing, network route, host route, directly attached route.
- IPv6 different routes like unicast, multicast, link-local, multicast link local. Link local routes can't be routed outside etc.
- Floating static routes, Longest prefix, administrative distance and metrics

# Subnetting

Octet -> bits in the given subnet (subnet: 10.3.4.77 /24, .77 is on 4th octet)

There are 256 addresses (bits) for each octet in a subnet xxx.xxx.xxx.xxx /xx

- when subtracting a .0 then adjust the leftside octet (bits)
- ex. 10.3.4.0 - 1 = 10.3.3.255

- See the best playlist for understanding subnetting: [https://youtu.be/BWZ-MHIhqjM?si=3YtRtNVwcbRyiQIh](https://youtu.be/BWZ-MHIhqjM?si=3YtRtNVwcbRyiQIh)
- Practice here with generated Target IP addresses: [http://pracnet.net/subnet](http://pracnet.net/subnet) or [https://subnetipv4.com/](https://subnetipv4.com/) (same website)

## Learn the subnetting trick

**NOTE that there are 7 informations to get in a subnet problem:**

1. Network ID
2. Broadcast IP
3. First Host IP
4. Last Host IP
5. Next Network
6. no. of (#) IP Addresses
7. CIDR/Subnet

_STEPS:_

1. Construct subnet table
2. Use given CIDR/mask to find column on the table
   a. **CIDR/Subnet** mask map to each other
   b. Locate _Group Size_
3. Get relevant octet range given the CIDR/Subnet
   c. Start at ".0" in relevant octet
   d. Increase by _Group Size_ until you PASS target IP

_THEN GET INFORMATION:_

- Number **BEFORE** target IP is _Network ID_
  - NOTE: octets (increased by group size in step 1) are all "Network IDs"
- Number **AFTER** target IP is _Next Network_
  - NOTE: if octet ended at .256 (range) then increment the last bit by 1
  - ex. from 10.3.3.198 /26 --> 10.3.4.0 will be the Next Network
    - since range is .192 to .256, cannot have .256 as Network ID for the Next Network
- IP address **BEFORE** Next Network is _Broadcast_ (Next Network - 1)
- IP address **AFTER** Network ID is _First Host_ (Network ID + 1)
- IP address **BEFORE** Broadcast IP is _Last Host_ (Broadcast IP - 1)
- Group size (if CIDR is on 4th octet) and 2^32-CIDR (if CIDR is on 3rd/2nd/1st octet) is total _# of IP addresses_
  - subtract 2 (Group size - 2) to know the "usable" IP addresses for hosts in the subnet
- CIDR/Notation, also known as Subnet Mask, is the 255 bits for each octet depending on the CIDR like 255.255.255.0
  - see the Subnet of the given CIDR and its octet

---

### Example Problems

Given: **198.242.237.119/26**

- _Network ID:_ ?
- _Broadcast IP:_ ?
- _First Host:_ ?
- _Last Host:_ ?
- _Next Network:_ ?
- _No. (#) of IP addresses:_ ?
- _CIDR/Notation:_ ?

---

#### 1: Construct Subnet Table

Steps:

- write the Group Size: '128 64 32 16 8 4 2 1'
- then get subnet by subtracting Group Size value to 256:
  - 256 - 128 = 128
  - 256 - 64 = 192
  - 256 - 32 = 224
  - and so on...
- then just list the octets like so:

_Group Size:_ 128 64 32 16 8 4 2 1  
_Subnet:_ 128 192 224 240 248 252 254 255  
_CIDR (4th octet):_ /25 /26 /27 /28 /29 /30 /31 /32  
_3rd octet:_ /17 /18 /19 /20 /21 /22 /23 /24  
_2nd octet:_ /9 /10 /11 /12 /13 /14 /15 /16  
_1st octet:_ /1 /2 /3 /4 /5 /6 /7 /8  

**Read the TABLE horizontally**

| **Group Size** | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| -------------- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Subnet**     | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
| **CIDR**       | /25 | /26 | /27 | /28 | /29 | /30 | /31 | /32 |
| **3rd octet**  | /17 | /18 | /19 | /20 | /21 | /22 | /23 | /24 |
| **2nd octet**  | /9  | /10 | /11 | /12 | /13 | /14 | /15 | /16 |
| **1st octet**  | /1  | /2  | /3  | /4  | /5  | /6  | /7  | /8  |

---

#### 2: Use given CIDR/mask to find column on the table

- **CIDR/Subnet** mask map to each other
- Locate _Group Size_

Given: **198.242.237.119/26**

| **Group Size** | 128 | **64**  | 32  | 16  | 8   | 4   | 2   | 1   |
| -------------- | --- | ------- | --- | --- | --- | --- | --- | --- |
| **Subnet**     | 128 | **192** | 224 | 240 | 248 | 252 | 254 | 255 |
| **CIDR**       | /25 | **/26** | /27 | /28 | /29 | /30 | /31 | /32 |
| **3rd octet**  | /17 | /18     | /19 | /20 | /21 | /22 | /23 | /24 |
| **2nd octet**  | /9  | /10     | /11 | /12 | /13 | /14 | /15 | /16 |
| **1st octet**  | /1  | /2      | /3  | /4  | /5  | /6  | /7  | /8  |

---

#### 3: Get relevant octet range given the CIDR/Subnet

* Given: **198.242.237.119/26**
* Group Size: **64**
* Octet: **4th octet**

- Start at ".0" in relevant octet
- Increase by _Group Size_ until you PASS target IP

  **198.242.237.119**  

**.119**  
.0 (same as 198.242.237.0)  
.64 (same as 198.242.237.64)  
.128 (same as 198.242.237.128)  
--> see .119 is between .64 and .128  

now get information in this order (for ease):

- CIDR/Notation
- Network ID
- Next Network
- Broadcast IP
- Last Host
- First Host
- No. (#) of IP addresses

**Finally:**

- Network ID: **198.242.237.64**
- Broadcast IP: **198.242.237.127**
- First Host: **192.242.237.65**
- Last Host: **192.242.237.126**
- Next Network: **198.242.237.128**
- No. (#) of IP addresses: **64**
- CIDR/Notation: **255.255.255.192**

---

## Speed Tips (for Subnetting)

Mostly applied on the 'relevant octet range' step
**NOTE: that when using this, make sure the range is accurate by its group size**

1. Multiplying by Group Size by 10 to get start Network ID
2. Doubling/Tripling Group Size to get start Network ID
3. Every Group Size lands on 128 at some point

- useful for subnet ranges' octets that is close to .128 (ex. 10.3.3.147 / 28 --> .147 is close to .128 so we can start counting by .128 already not by .0)

4. Every group size lands on the SUBNET of its SAME COLUMN and EVERY COLUMN TO THE LEFT

- ex. 10.3.3.197 /30 --> can start at .192 since:

| **Group Size** | 128     | 64      | 32      | 16      | 8       | **4**   | 2   | 1   |
| -------------- | ------- | ------- | ------- | ------- | ------- | ------- | --- | --- |
| **Subnet**     | **128** | **192** | **224** | **240** | **248** | **252** | 254 | 255 |
| **CIDR**       | /25     | /26     | /27     | /28     | /29     | **/30** | /31 | /32 |
| **3rd octet**  | /17     | /18     | /19     | /20     | /21     | /22     | /23 | /24 |
| **2nd octet**  | /9      | /10     | /11     | /12     | /13     | /14     | /15 | /16 |
| **1st octet**  | /1      | /2      | /3      | /4      | /5      | /6      | /7  | /8  |

5. Start high, then subtract

---

# IEEE Standards for 802.1xx

802.11 - it has only 2 1’s with no letters, so it’s all “2’s”, 2.4 GHz, 2 Mbps

802.11a - "advanced" band 5 GHz, "adequate" speed 54 Mbps.

802.11b - “bad everything”. 2.4 GHz is slower than 5 GHz so it’s the "bad band", and 11 Mbps is “bad speed” cause it’s not much of an improvement from 802.”11”

802.11g - the “OG” band 2.4 GHz, "grim" speed 54 Mbps

802.11n - 2.4 GHz “n” 5 GHz, "new" speed 600 Mbps

802.11ac - "Advanced (a)" band 5 GHz, "Commendable (c)" speed 6.93 Gbps

802.11ax - "All (a)" bands, 2.4 / 5 / 6 GHz, "Xtreme (x)" speed 4 \* 6.93 Gbps

I'm not exactly good at mnemonics tbh but it helps me so I thought I'd share

https://github.com/psaumur/CCNA_Course_Notes 
