---
title: About
pcx-content-type: concept
weight: 2
meta:
  title: About Magic Transit
---

# About Magic Transit

Magic Transit delivers its connectivity, security, and performance benefits by serving as the “front door” to your IP network. This means it accepts IP packets destined for your network, processes them, and then outputs them to your origin infrastructure.

The Cloudflare network uses Border Gateway Protocol (BGP) to announce your company’s IP address space, extending your network presence globally, and Cloudflare uses [Anycast](https://www.cloudflare.com/learning/cdn/glossary/anycast-network/) to ingest your traffic. Today, Cloudflare’s Anycast edge network spans 250 cities in more than 100 countries around the world.

Once packets hit Cloudflare’s network, traffic is inspected for attacks, filtered, steered, accelerated, and sent onward to your origin. Magic transit connects to your origin infrastructure using Anycast Generic Routing Encapsulation (GRE) tunnels over the Internet or, with [Cloudflare Network Interconnect (CNI)](/network-interconnect/), via physical or virtual interconnect.

For more on how Magic Transit works, review the information below.

{{<directory-listing>}}
