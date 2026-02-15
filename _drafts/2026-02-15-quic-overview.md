---
title: "QUIC Overview"
layout: reveal
date: "2026-02-15"
# permalink: posts/wordpress-to-static-site/
categories: []
tags: []
# img_path: /assets/images/wordpress-to-static-site/
# image: thumb.png
# https://benswift.me/blog/2018/09/28/another-reveal.js-plugin-for-jekyll/
---

## Jarrod Explains QUIC

- What is QUIC?
- QUIC vs HTTP3?
- What Problems Does it Solve?
- History
- Technical Overview
- Use Cases
- What its good for, what its not good for and alternatives
- Code demo - unity, c# quic client and quic server deployed into vps, maybe using terraform / ansible
- Packet capture example

## What is QUIC?

- Transport layer protocol

## What problems does QUIC solve?

- Problems with TCP
  - Head of line blocking
  - Inefficient handshakes (especially with TLS)
  - Protocol ossification - it is very difficult to introduce new transport protocols because networking devices such as firewalls, network address translators, load balancers, and deep packet inspection (aka as middleboxes) have a big problem with dropping packets in a format they are not familar with. A backwards compatibility problem.

## QUIC Addressing Head of Line Blocking Problem

## QUIC Addressing Inefficient Handshake Problem

## QUIC Addressing Protocol Ossification Problem

## QUIC use cases

- HTTP3
- Low latency datagrams in the browser with WebTransport + QUIC

## Demo

- Unity multiplayer game
- Backend QUIC Server using Golang
- WebGL using WebTransport with QUIC
- Desktop and Mobile builds using dotnet System.Net.Quic
- Packet capture demo

