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

In a URL shortening system (commonly asked in a technical interview), a client can be a website similar to [bitly.com](https://bitly.com), which interacts with users who wants to shorten a long link in his hand. Once a user enters his link in the text box and click the `Shorten` button, a http request will be sent to a remote server with the link to be shortened and optionally the user’s information if she/he logins in. In a small system, the server might just be a single machine that listens a TCP/IP port and exchanges data with outside world. The server processes the request, namely shortening the provided link by using some common techniques like hashing, and store relevant data in a database that can be relational database like MySQL, PostgresSQL or non-relational database such as MongoDB, Cassandra, etc. In a large system that serves millions of users, the server is actually a distributed system that probably comprise of load balancer, tons of commodity machines running the same web service. Additionally, a caching layer will be placed in an appropriate position (we shall revisit more details later about caching system). There will be clusters of database instances which are either leader(s) or followers (a side note: I will avoid using master/slave term as much as possible, but they are the same concepts as leader/follower.). 


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMwMDc4Mzc5MSwxODk0Mjg3MjUsMTMwMz
QzOTQyOSwtOTYwMDczMzMyLC02MjI3ODU4MjIsMjA1MzY2OTQy
OSw1MTY3Mjc2MDUsLTQ0NzUwMDIxMiwtMTA4NTgyNjE1LC0xMD
g1ODI2MTUsLTY4NTkyNDYzN119
-->