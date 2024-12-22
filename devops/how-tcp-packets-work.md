---
date: 2024-09-11T14:10
tags:
  - Infra
---

<!-- 2024-09-11-1410 (September 11, 2024 02:10:50 PM) -->

# How TCP Packets Work?

first, we don't need to exactly know how TCP works to know how TCP packets work

- but we use TCP to offload the handling of the reliability, orderness, checksum of the packets we send
- **we only need to handle the "meaning" of the packets (content)**
- MTU (Maximum Transmission Unit) -> maximum amount/measurement of bytes we can send and receive from/to the internet
- TCP's MSS that determines the segment size...still very much subject to MTU at the L2/Ethernet layer and any other encapsulation headers involved though

imagine a json body (10K)

- we need to parse this as byte

What do we need to handle?

1. Reading the Packets (from left to right) -> using [Big Endian](https://www.freecodecamp.org/news/what-is-endianness-big-endian-vs-little-endian/)
1. Header ordering of the packet

   - It should be in order:
     - Version
     - Command -> how i need to parse the rest of the data
     - Length -> 2 bytes (max) to represent/accomodate 65K (65K is large)
     - Data

   ```txt
   [ [ Version ] | [ Command ] | [ Length ] | [     Data     ] ]

   as slice in go: [ Version, Command, Length, Data ]
   ```

   - no need to put one byte addative checksum at the end _in TCP_

## Unmarshalling bytes (parse from json to 'object code')

- 2 bytes length

## Marshalling bytes (parse from 'object code' to json)

- just reverse the process of unmarshal
- create new packet (byte slice/array)
- then append each by order:
  - Version
  - Command
  - Length
  - Data
- return the whole packet (byte slice/array)

## Read the data of the sent packets

- all the above explanation is just for parsing bytes
- infinite loop through the sent bytes to `Read()` the whole packet
  - byte per byte (byte size to read is handled by Big Endian)
- append the previous bytes read to a new byte slice/array
  - after appending all, then we can process that as a whole packet

# Resources

- [This Perfect Explanation by Prime](https://youtu.be/4NjE86ur-ck?si=6dFzrSSmcbl0s4H9)
- [see vim-with-me go library by Prime in GitHub]()
