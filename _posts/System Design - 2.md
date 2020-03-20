---
layout: post
comments: true
categories: system-design
title: System Design - Part 2: Where do we start
---
Before we dig into various aspects of system designs, we shall first review and retrospect where we start when asked for designing a system. 

We can start with a high-level architecture, similar to Figure-1 below. Before long, you will find that nearly majority of systems you are about to design follow the same pattern. A client sends a request to a server, and then the server queries a database. The database sends back some data or confirmation and the server optionally processes the data before sending a response to the client. 

![A Simple System Design to Start](assets/5DE69707-4217-43FB-A7DF-C9ED96E0A99E.jpeg)

This diagram seems specific to an online system, for example a website, but can be applied to many problems if you think of the `client`, `server`, `database` in a more general way. 

> `client` can be: a real user, a web browser, a programming language library, a console or terminal, a batch job driver
> `server` can be: any server that accepts requests from clients and responds accordingly. For example, a web server, a load balancer, a DNS server, a cache system, a batch job master server
> `database` can be: any system that persists any data for future usage

Let’s review some examples.

In a URL shortening system (commonly asked in a technical interview), a client can be the website 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMzg1Nzk1OTUsLTYyMjc4NTgyMiwyMD
UzNjY5NDI5LDUxNjcyNzYwNSwtNDQ3NTAwMjEyLC0xMDg1ODI2
MTUsLTEwODU4MjYxNSwtNjg1OTI0NjM3XX0=
-->