---
layout: post
comments: true
categories: system-design
title: System Design - Part 2: Where do we start
---
Before we dig into various aspects of system designs, we shall first review and retrospect where we start when asked for designing a system. 

We can start with a high-level architecture, similar to Figure-1 below. Before long, you will find that nearly majority of systems you are about to design follow the same pattern. A client sends a request to a server, and then the server queries a database. The database sends back some data or confirmation and the server optionally processes the data before sending a response to the client. This diagram seems specific to an online system, for example a website, but can be applied to many problems if you think of the `client`, `server`, `database` in a more general way. 

> `client` can be: a real user, a web browser, a programming language library, a console or terminal, a batch job

![A Simple System Design to Start](assets/5DE69707-4217-43FB-A7DF-C9ED96E0A99E.jpeg)
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTE2NzI3NjA1LC00NDc1MDAyMTIsLTEwOD
U4MjYxNSwtMTA4NTgyNjE1LC02ODU5MjQ2MzddfQ==
-->