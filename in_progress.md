# ds_notes

Balance and propensity score

- [ ] [model selection](https://towardsdatascience.com/a-short-introduction-to-model-selection-bb1bb9c73376)

- [ ] [machine learning systems design](https://github.com/chiphuyen/machine-learning-systems-design/blob/master/build/build1/consolidated.pdf)

- [ ] Look for ranking algorithms
measure the effect of the treatment on the outcome -> what is the effect of newsfeed on the engagement

**average treatment effect** = average on outcome of treated population - average on outcome of non-treated population. the average treatment effect of showing friend activity in the news feed, after controling for gender, age and location, is 5 visits a month

counterfactuals (similar to missing data problem). 

observational studies are non-randomized experiments. there might be confounding: a bias to get the treatment and bias to get the desired outcome

**unconfounded test after controlling**

fine-tune an interface so everyone can understand it

When you do a test, significance can be used
When you do several tests, significance can't be used because by definition: if you do one test you have 95% confidence of your result. If you do ten tests then you can't make the same claim, that is you won't have 95% confidence.
When you want to do several tests, you should try multiple testing adjustment.



there is no evidence of the treatment having an effect 

bootstrap has less power, is a conservative test. It would only reject if you have a lot of evidence in the data
power is the ability to reject the null when the null is false, when the alternative is true
a t-test has more power, it will more likely reject the null hypothesis when it is actually false 
type error 1
type error 2
don't reject when you should reject
control over type error 1 with low probability of type error 2

# SQL

## What is an index?

A database index is a data structure that provides quick lookup of data in a column or columns of a table. It enhances the speed of operations accessing data from a database table at the cost of additional writes and memory to maintain the index data structure.

Creating an index on a field in a table creates another data structure which holds the field value, and a pointer to the record it relates to. This index structure is then sorted, allowing Binary Searches to be performed on it.

The downside to indexing is that these indices require additional space on the disk##  





