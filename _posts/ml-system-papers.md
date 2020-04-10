---
layout: post
comments: true
categories: resources
title: ML System Papers You Should Read
---

Machine learning system is a specialized system that is built for solving problems with machine learning deeply integrated. While it has existed for a while, machine learning is just becoming popular in recent decades compared with other technologies. 

As more machine learning based solutions are deployed in production, the system around ML has involved as well. Majority of ML systems are tackling specific problems such as personalization, recommendation and ads ranking. There is also a trend in generalized machine learning system that solves a variety of problems. To learn how a machine learning system is built, the best way is probably through practice with real-life projects. The second best way are reading papers published by well-known companies that specialize in applied machine learning. Below is a list of good papers that I personally have learned a lot from, and hopefully you will distill useful knowledge for your interests.

[TFX: A TensorFlow-Based Production-Scale Machine Learning](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/b500d77bc4f518a1165c0ab43c8fac5d2948bc14.pdf)

> ABSTRACT
> Creating and maintaining a platform for reliably producing and
> deploying machine learning models requires careful or- chestration of
> many components—a learner for generating models based on training
> data, modules for analyzing and val- idating both data as well as
> models, and finally infrastructure for serving models in production.
> This becomes particularly challenging when data changes over time and
> fresh models need to be produced continuously. Unfortunately, such or-
> chestration is often done ad hoc using glue code and custom scripts
> developed by individual teams for specific use cases, leading to
> duplicated effort and fragile systems with high technical debt.

[Practical Lessons from Predicting Clicks on Ads at Facebook](https://research.fb.com/wp-content/uploads/2016/11/practical-lessons-from-predicting-clicks-on-ads-at-facebook.pdf?)

>ABSTRACT
> Online advertising allows advertisers to only bid and pay for
> measurable user responses, such as clicks on ads. As a consequence,
> click prediction systems are central to most on- line advertising
> systems. With over 750 million daily active users and over 1 million
> active advertisers, predicting clicks on Facebook ads is a challenging
> machine learning task. In this paper we introduce a model which
> combines decision trees with logistic regression, outperforming either
> of these methods on its own by over 3%, an improvement with sig-
> nificant impact to the overall system performance. We then explore how
> a number of fundamental parameters impact the final prediction
> performance of our system. Not surpris- ingly, the most important
> thing is to have the right features: those capturing historical
> information about the user or ad dominate other types of features.
> Once we have the right features and the right model (decisions trees
> plus logistic re- gression), other factors play small roles (though
> even small improvements are important at scale). Picking the optimal
> handling for data freshness, learning rate schema and data sampling
> improve the model slightly, though much less than adding a high-value
> feature, or picking the right model to begin with.

[Predictive model performance: offline and online evaluations](https://dl.acm.org/doi/10.1145/2487575.2488215)
[Wide & Deep Learning for Recommender Systems](https://arxiv.org/pdf/1606.07792.pdf)
[Deep Neural Networks for YouTube Recommendations](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45530.pdf)
[Factorization Machines](https://cseweb.ucsd.edu/classes/fa17/cse291-b/reading/Rendle2010FM.pdf)
[SlowFast Networks for Video Recognition](https://arxiv.org/pdf/1812.03982.pdf)
[Searching for Communities: a Facebook Way](https://research.fb.com/wp-content/uploads/2019/06/Searching-for-Communities-a-Facebook-Way.pdf?)


**Some paper-like articles**
[Rules of Machine Learning: Best Practices for ML Engineering](http://martin.zinkevich.org/rules_of_ml/rules_of_ml.pdf)
[Meet Michelangelo: Uber’s Machine Learning Platform](https://eng.uber.com/michelangelo-machine-learning-platform/)
[Introducing FBLearner Flow: Facebook’s AI backbone](https://engineering.fb.com/core-data/introducing-fblearner-flow-facebook-s-ai-backbone/)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNjE3NjAzNzgsNjc4NzYwNzE0LC0zMT
E4MjMwNzcsLTE0NDYyOTI3ODYsLTg4MzkxNTgzNSwtMTg2ODE2
MDg2NF19
-->