---
date: 2024-09-11T14:06
tags:
  - Infra
---

<!-- 2024-09-11-1406 (September 11, 2024 02:06:59 PM) -->

# TCP HTTP HTTPS Differences

## TCP

TCP handles the guarantees that you'd want to have to deem your communication is reliable.

- It guarantees in-order delivery, retransmission of dropped packets, and de-duplication of packets.

## HTTP

HTTP is concerned with the contents of the communication between two parties.

- Like, if I introduce myself "what's your name", convention is to respond with your name and maybe a gesture like a handshake.

In the same vein, there are always required headers from the client like the HTTP method, and even if the server doesn't respond with a body,

- it still needs to respond with a status code like 200, 303, 404, 500 so the client ( usually a web browser ) knows how to process that.

In order for clients and servers to be able to understand each other, there have to be some defined elements that are always present in each message, and HTTP governs what those parts are.

- In other words, **HTTP defines how what plain text communication should be sent between two parties in order to have a meaningful conversation.**

## HTTPS

As for HTTPS, that just means your HTTP connection is using TLS ( transport layer security).

One of the 3 reasons is: if someone is packet sniffing it doesn't matter because the message has been encrypted.

The other two parts is for clients to be able to trust that the server they're making a connection with IS ACTUALLY the entity that they say they are ( what good is encrypting messages if I'm unknowingly connected to a bad website.

The last part of TLS is that you have guarantees that your message wasn't tampered with ( think of the tamper seals on food, for example ).

## The short version of this is:

**TCP**: how do we deliver data "reliably" over an unreliable medium

**HTTP**: what kind of structure / form should our text messages between us take?

**HTTPS (TLS)**: how do we make this data transfer secure?
