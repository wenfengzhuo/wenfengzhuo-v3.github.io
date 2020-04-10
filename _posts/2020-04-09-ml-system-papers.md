---
layout: post
comments: true
categories: resources
title: ML System Papers You Should Read
---

Machine learning system is a specialized system that is built for solving problems with machine learning deeply integrated. While it has existed for a while, machine learning is just becoming popular in recent decades compared with other technologies. 

As more machine learning based solutions are deployed in production, the system around ML has evolved as well. Majority of ML systems are tackling specific problems such as personalization, recommendation and ads ranking. There is also a trend in generalized machine learning system that solves a variety of problems. To learn how a machine learning system is built, the best way is probably through practice with real-life projects. The second best way is by reading papers published by well-known companies that specialize in applied machine learning. 

Below is a list of good papers that I personally have learned a lot from, and hopefully you will distill useful knowledge for your interests.

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

> ABSTRACT
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

> ABSTRACT
> We study the accuracy of evaluation metrics used to estimate the
> efficacy of predictive models. Offline evaluation metrics are
> indicators of the expected model performance on real data. However, in
> practice we often experience substantial discrepancy between the
> offline and online performance of the models.
> 
> We investigate the characteristics and behaviors of the evaluation
> metrics on offline and online testing both analytically and
> empirically by experimenting them on online advertising data from the
> Bing search engine. One of our findings is that some offline metrics
> like AUC (the Area Under the Receiver Operating Characteristic Curve)
> and RIG (Relative Information Gain) that summarize the model
> performance on the entire spectrum of operating points could be quite
> misleading sometimes and result in significant discrepancy in offline
> and online metrics. For example, for click prediction models for
> search advertising, errors in predictions in the very low range of
> predicted click scores impact the online performance much more
> negatively than errors in other regions. Most of the offline metrics
> we studied including AUC and RIG, however, are insensitive to such
> model behavior.
> 
> We designed a new model evaluation paradigm that simulates the online
> behavior of predictive models. For a set of ads selected by a new
> prediction model, the online user behavior is estimated from the
> historic user behavior in the search logs. The experimental results on
> click prediction model for search advertising are highly promising.

[Wide & Deep Learning for Recommender Systems](https://arxiv.org/pdf/1606.07792.pdf)

> ABSTRACT
> Generalized linear models with nonlinear feature transfor- mations are
> widely used for large-scale regression and clas- sification problems
> with sparse inputs. Memorization of fea- ture interactions through a
> wide set of cross-product feature transformations are effective and
> interpretable, while gener- alization requires more feature
> engineering effort. With less feature engineering, deep neural
> networks can generalize bet- ter to unseen feature combinations
> through low-dimensional dense embeddings learned for the sparse
> features. However, deep neural networks with embeddings can
> over-generalize and recommend less relevant items when the user-item
> inter- actions are sparse and high-rank. In this paper, we present
> Wide & Deep learning—jointly trained wide linear models and deep
> neural networks—to combine the benefits of mem- orization and
> generalization for recommender systems. We productionized and
> evaluated the system on Google Play, a commercial mobile app store
> with over one billion active users and over one million apps. Online
> experiment results show that Wide & Deep significantly increased app
> acquisi- tions compared with wide-only and deep-only models. We have
> also open-sourced our implementation in TensorFlow.

[Deep Neural Networks for YouTube Recommendations](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45530.pdf)

> ABSTRACT
> YouTube represents one of the largest scale and most sophis- ticated
> industrial recommendation systems in existence. In this paper, we
> describe the system at a high level and fo- cus on the dramatic
> performance improvements brought by deep learning. The paper is split
> according to the classic two-stage information retrieval dichotomy:
> first, we detail a deep candidate generation model and then describe a
> sepa- rate deep ranking model. We also provide practical lessons and
> insights derived from designing, iterating and maintain- ing a massive
> recommendation system with enormous user- facing impact.

[Factorization Machines](https://cseweb.ucsd.edu/classes/fa17/cse291-b/reading/Rendle2010FM.pdf)

> ABSTRACT 
> In this paper, we introduce Factorization Machines (FM) which
> are a new model class that combines the advantages of Support Vector
> Machines (SVM) with factorization models. Like SVMs, FMs are a general
> predictor working with any real valued feature vector. In contrast to
> SVMs, FMs model all interactions between variables using factorized
> parameters. Thus they are able to estimate interactions even in
> problems with huge sparsity (like recommender systems) where SVMs
> fail. We show that the model equation of FMs can be calculated in
> linear time and thus FMs can be optimized directly. So unlike
> nonlinear SVMs, a transformation in the dual form is not necessary and
> the model parameters can be estimated directly without the need of any
> support vector in the solution. We show the relationship to SVMs and
> the advantages of FMs for parameter estimation in sparse settings.
> 
> On the other hand there are many different factorization mod- els like
> matrix factorization, parallel factor analysis or specialized models
> like SVD++, PITF or FPMC. The drawback of these models is that they
> are not applicable for general prediction tasks but work only with
> special input data. Furthermore their model equations and optimization
> algorithms are derived individually for each task. We show that FMs
> can mimic these models just by specifying the input data (i.e. the
> feature vectors). This makes FMs easily applicable even for users
> without expert knowledge in factorization models.

[SlowFast Networks for Video Recognition](https://arxiv.org/pdf/1812.03982.pdf)

> ABSTRACT
> We present SlowFast networks for video recognition. Our model involves
> (i) a Slow pathway, operating at low frame rate, to capture spatial
> semantics, and (ii) a Fast path- way, operating at high frame rate, to
> capture motion at fine temporal resolution. The Fast pathway can be
> made very lightweight by reducing its channel capacity, yet can learn
> useful temporal information for video recognition. Our models achieve
> strong performance for both action classification and detection in
> video, and large improve- ments are pin-pointed as contributions by
> our SlowFast con- cept. We report state-of-the-art accuracy on major
> video recognition benchmarks, Kinetics, Charades and AVA. Code has
> been made available at: https://github.com/facebookresearch/SlowFast.

[Searching for Communities: a Facebook Way](https://research.fb.com/wp-content/uploads/2019/06/Searching-for-Communities-a-Facebook-Way.pdf?)

> ABSTRACT
> Giving people the power to build community is central to Face- book’s
> mission. Technically, searching for communities poses very different
> challenges compared to the standard IR problems. First, there is a
> vocabulary mismatch problem since most of the content of the
> communities is private. Second, the common labeling strategies based
> on human ratings and clicks do not work well due to limited public
> content available to third-party raters and users at search time.
> Finally, community search has a dual objective of satisfying searchers
> and growing the number of active communities. While A/B testing is a
> well known approach for assessing the former, it is an open question
> on how to measure progress on the latter. This talk discusses these
> challenges in depth and describes our solution.

**Some paper-like articles**

[Rules of Machine Learning: Best Practices for ML Engineering](http://martin.zinkevich.org/rules_of_ml/rules_of_ml.pdf)

[Meet Michelangelo: Uber’s Machine Learning Platform](https://eng.uber.com/michelangelo-machine-learning-platform/)

[Introducing FBLearner Flow: Facebook’s AI backbone](https://engineering.fb.com/core-data/introducing-fblearner-flow-facebook-s-ai-backbone/)

***Note: Please use the link of the article to share the content rather than any form of copy-paste.***
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg0NjQzOTAxM119
-->