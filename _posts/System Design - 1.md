## System Design - Part 1
<hr/>

System Design is an essential skill for every software engineer. The magnitude of involvement will differ from person to person based on their experience and seniority. It is also critically important in the interview evaluation process. It shows how well you understand software system and computer science applied to real-life problems.

While abundant resource about how to be an expert on system design, it still remains as a difficult skill for engineers to obtain. Usually, people are biased by years of experience when judging your system design capability. More extremely, some people might not believe you are able to design a complicated system without years of experience above a threshold. 

It’s indeed intricate but not without possibility for us to excel it even with few practical experience. If we can derive some common patterns in designing a robust system, we should be able to leverage them to come up with novel solutions. We usually see in many well-design systems, there are “trade-offs” all over the place. Trade-off is when you trade A with B to maximize your win. 

The key here is to determine whether it’s A or B you want to trade. It will be a function of many factors: the problem you are solving, the resource you have, the timeline, the cost of solutions, the users the system is serving, the community, the culture of your company, etc.. These factors ultimately make system design a difficult problem to tackle. Essentially in my point of view, it’s a “trade-off science”.

 Another key element in this “trade-off science” is the ability to identify the A and the B in your system that you must choose between. Usually it comes with the pattern: if I choose A, B will suffer; or if I select B, A will be compromised. A and B can be very specific to the system you are designing, but there exists a quite number of A and B in most of systems. In fact, those specific to your system might be just derivations of the generic factors. For example, in a distributed system,  we have often heard of the terms: availability, consistency, latency. They are examples of the A and B.

With these two areas to focus, we can attempt to generalize the approaches to system design. We can start off with a discussion about the common factors of a system design and then some common ways to make the trade-offs among those factors. In the following series of articles, I will attempt to cover several topics about system design. Hopefully it will shed some lights on systematic system design for y



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgyMjU5ODcxNCwtNDQzNjY2ODk0LDU0MD
ExNDE0OSwtODczNTQ5NTM1LC02NjEzMDI0NTUsMTQ5NTY0MDE3
NywtNjMyNzg1Mzk1XX0=
-->