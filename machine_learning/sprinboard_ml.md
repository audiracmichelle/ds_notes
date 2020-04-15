# springboard_ml

41 Essential Machine Learning Interview Questions from Springboard [https://www.springboard.com/blog/machine-learning-interview-questions/](https://www.springboard.com/blog/machine-learning-interview-questions/)

## Question 1: What’s the trade-off between bias and variance?

We use the bias variance trade-off **to understand if a model generalizes well** in unseen data. A model that is either too complex or too simple might not generalize well.

I like to think of a **dartboard** when thinking of the bias-variance trade-off. Imagine you have two people throwing darts. The first person throws ten darts and all the darts hit an area far from the target but are very close from each other. This throwers aim is very biased and has little variance. The second thrower hits the target once but the other nine darts are spread all over the dart board. The second thrower's aim has a lot of variance but it's not biased.     

In modeling, in lots of cases, the output is an estimate of something. In order for this estimate to be good, it should be close to the real value of the thing we're trying to model. During the modeling process, the idea of bias-variance trade off **helps us assess how to make decisions** and get more -a model that generalizes well- reliable estimates.

When the training set is too large, or we are using too many variables, we have reason to believe that we are **overfitting** and therefore our estimates will have a lot of variance once we the input a different dataset.

When the training set is too small, or we are not using enough covariates, what could happen is that we are **underfitting** and our estimates are too far away from the target values and that would mean that they are unbiased.

Interestingly enough, there can be high-bias high-variance estimators as well as low-bias low-variance estimators (which are most desirable estimators).

- [x] Andrew Ng's explanation of bias variance trade-off [https://www.youtube.com/watch?v=SjQyLhQIXSM](https://www.youtube.com/watch?v=SjQyLhQIXSM)

### Question 2: What is the difference between supervised and unsupervised learning?

**Supervised learning** covers methods where there is a set of known labels or values that we want to obtain by learning **their relationship with other variables**. Examples of supervised learning are regression and classification.

**Unsupervised methods** **look for patterns** or the **natural structure** that help us describe a population. Unsupervised learning is very useful in exploratory analysis because it can automatically identify structure in data. Examples of unsupervised learning methods are clustering, dimensionality reduction and density estimation.

![](./supervised_unsupervised.png)

**Representation Learning** word2vec, neural networks

- [x] Towards Data Science post on superset and unsupervised learning [https://towardsdatascience.com/supervised-vs-unsupervised-learning-14f68e32ea8d](https://towardsdatascience.com/supervised-vs-unsupervised-learning-14f68e32ea8d)

### Question 3: What is the difference between k-nn and k-means?

**K-nn** is a simple supervised algorithm. Because it is supervised it can be used for classification or regression:

As a classifier it works like a **class membership**. A given point is classified by a **plurality vote** of its k nearest neighbors. 

As a regressor the output of a given point is **the average of the output labels** of its k nearest neighbors.

It is commonly used to **benchmark** other models but because it is instance-based (it "memorizes" and uses all its training datapoints) and that comes with a cost, it is uses a lot of memory.

It can also work for imputation of missing data. They would be a good choice for problems that have graph structured data. For example an application could be recommendation systems in social graphs.

If k=1 then we would be overfitting and would have a lot of flexibility, that is a low-bias high-variance estimator
If k is too big, the **decision curve** is smoother and we get a high-bias low-variance estimator.

Drawback: **curse of dimensionality** therefore it is desirable to use dimensionality reduction first.

To determine the k use cross-validation k-fold cross validation.

- [x] [https://kevinzakka.github.io/2016/07/13/k-nearest-neighbor/]https://kevinzakka.github.io/2016/07/13/k-nearest-neighbor/
- [ ] [https://medium.com/30-days-of-machine-learning/day-3...](https://medium.com/30-days-of-machine-learning/day-3-k-nearest-neighbors-and-bias-variance-tradeoff-75f84d515bdb)

**K-means** is an unsupervised algorithm


- [x] drawbacks [http://varianceexplained.org/r/kmeans-free-lunch/](http://varianceexplained.org/r/kmeans-free-lunch/)
- [x] algorithm [https://bigdata-madesimple.com/possibly-the-simplest-way-to-explain-k-means-algorithm/](https://bigdata-madesimple.com/possibly-the-simplest-way-to-explain-k-means-algorithm/)
- [x] overview [https://towardsdatascience.com/k-means-...](https://towardsdatascience.com/k-means-clustering-algorithm-applications-evaluation-methods-and-drawbacks-aa03e644b48a)


### Question 4: Why is “Naive” Bayes naive?

### Question 5: What is Bayes’ Theorem? How is it useful in a machine learning context?

https://www.youtube.com/watch?v=bTs-QA2oJSE
https://medium.com/30-days-of-machine-learning/day-6-naive-bayes-classifier-9b8bacb3a2aa

















 

   


